NUGG_SALES_ORDER contains

class ZCL_SALES_ORDER

	static method GET_INSTANCE

		This method returns an instance of ZCL_SALES_ORDER
		
	instance method SET_DELIVERY_BLOCK
	
		This method sets the delivery block in the sales order
		
	instance method GET_PARTNER
	
		This method returns a partner of type VBPA

	instance method INIT_DATA
	
		This method will initialize/refresh the instance data

	instance method INIT_PARTNERS
	
		This method will initialize/refresh the instance partner data



class ZCX_SALES_ORDER

	Exception class for Sales Order related exceptions


class ZCX_SALES_ORDER_NOT_FOUND

	Sales Order not found class exception
