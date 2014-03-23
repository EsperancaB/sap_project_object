*pre-requisites*
These classes use objects from:
NUGG_TRANSFER_ORDER


NUGG_DELIVERY contains

class ZCL_DELIVERY_ITEM

	public static method GET_INSTANCE

		This method returns an instance of ZCL_DELIVERY_ITEM

	protected instance method INIT_DOC_FLOW

		This method populates the document flow tables, subsequent and precendent documents

	public instance method GET_TRANSFER_ORDER

		This method retrieves the Transfer Order related to the respective delivery item
		from the subsequent documents table


class ZCX_DELIVERY

	Exception class for Delivery related exceptions


class ZCX_DELIVERY_ITEM

	Exception class for Delivery item related exceptions


class ZCX_DELIVERY_ITEM_NOT_FOUND

	Delivery item not found class exception