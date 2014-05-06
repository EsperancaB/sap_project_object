*&---------------------------------------------------------------------*
*& Report  ZESY_CALL_OO_BDC
*&
*&---------------------------------------------------------------------*
REPORT  zesy_call_oo_bdc.

*----------------------------------------------------------------------*
*  Selection Screen
*----------------------------------------------------------------------*
PARAMETER:
  p_kunnr  TYPE kna1-kunnr DEFAULT '2400',
  p_vkorg  TYPE knvv-vkorg DEFAULT '1000',
  p_vtweg  TYPE knvv-vtweg DEFAULT '10',
  p_spart  TYPE knvv-spart DEFAULT '00'.
SELECTION-SCREEN SKIP 1.
PARAMETER:
  p_delblk TYPE kna1-lifsd DEFAULT '01',
  p_bilblk TYPE kna1-faksd DEFAULT '01'.
SELECTION-SCREEN SKIP 1.

PARAMETER:
  p_tran RADIOBUTTON GROUP r1 DEFAULT 'X',
  p_sess RADIOBUTTON GROUP r1.

*----------------------------------------------------------------------*
*  Data Declarations
*----------------------------------------------------------------------*
DATA:
  g_bdc       TYPE REF TO zcl_bc_bdc,
  g_messages  TYPE tab_bdcmsgcoll,
  g_message   LIKE LINE OF g_messages,
  g_subrc     TYPE sy-subrc,
  g_exception TYPE REF TO zcx_batch_input_error,
  g_error     TYPE string.

*----------------------------------------------------------------------*
*  Initialization
*----------------------------------------------------------------------*
*INITIALIZATION.

*----------------------------------------------------------------------*
*  Start of Selection
*----------------------------------------------------------------------*
START-OF-SELECTION.

* Create an object for BDC
  IF p_tran = 'X'.
    g_bdc = zcl_bc_bdc=>s_instantiate( im_bdc_type = 'CT'
                                       im_tcode    = 'VD05' ).
  ELSEIF p_sess = 'X'.
    g_bdc = zcl_bc_bdc=>s_instantiate( im_bdc_type = 'BI'
                                       im_tcode    = 'VD05'
                                       im_group    = 'MY_SESSION'
                                       im_keep     = 'X'
                                       im_holddate = '20140528' ).
  ENDIF.

* Populate first screen
  g_bdc->add_screen( im_repid = 'SAPMF02D' im_dynnr = '0507').
  g_bdc->add_field( im_fld = 'BDC_OKCODE'  im_val = '/00').
  g_bdc->add_field( im_fld = 'RF02D-KUNNR' im_val = p_kunnr ).
  g_bdc->add_field( im_fld = 'RF02D-VKORG' im_val = p_vkorg ).
  g_bdc->add_field( im_fld = 'RF02D-VTWEG' im_val = p_vtweg ).
  g_bdc->add_field( im_fld = 'RF02D-SPART' im_val = p_spart ).
* Populate second screen
  g_bdc->add_screen( im_repid = 'SAPMF02D' im_dynnr = '0510').
  g_bdc->add_field( im_fld = 'BDC_OKCODE' im_val = '=UPDA').
  g_bdc->add_field( im_fld = 'KNA1-LIFSD' im_val = p_delblk ).
  g_bdc->add_field( im_fld = 'KNA1-FAKSD' im_val = p_bilblk ).

*----------------------------------------------------------------------*
*  End of Selection
*----------------------------------------------------------------------*
END-OF-SELECTION.

  IF p_tran = 'X'.
    g_bdc->process( IMPORTING ex_subrc    = g_subrc
                              ex_messages = g_messages ).
*   Display processing status
    IF g_subrc = 0.
      WRITE:/ 'Success:'.
    ELSE.
      WRITE:/ 'Error:'.
*     Display messages in list
      LOOP AT g_messages INTO g_message.
        MESSAGE ID g_message-msgid
                TYPE g_message-msgtyp
                NUMBER g_message-msgnr
                WITH g_message-msgv1
                     g_message-msgv2
                     g_message-msgv3
                     g_message-msgv4
                INTO g_error.
        WRITE:/ g_error.
      ENDLOOP.
    ENDIF.
  ELSEIF p_sess = 'X'.
    TRY.
        g_bdc->process( ).
        WRITE: 'Success: Check session in SM35'.
      CATCH zcx_batch_input_error INTO g_exception.
        WRITE:/ 'Error creating batch input session:'.
        MESSAGE ID g_exception->msgid
                TYPE 'E'
                NUMBER g_exception->msgno
                WITH g_exception->msgv1
                     g_exception->msgv2
                     g_exception->msgv3
                     g_exception->msgv4
                INTO g_error.
        WRITE:/ g_error.
    ENDTRY.
  ENDIF.
