---
# required metadata

title: Stock transfer orders for India
description:  This article provides information about the stock transfer functionality that is available for India in Microsoft Dynamics 365 Finance.
author: EvgenyPopovMBS
ms.date: 08/05/2019
ms.topic: article
ms.prod: 
ms.technology: 

# optional metadata

ms.search.form: InventTransferOrders, ReqParameters
audience: Application User
# ms.devlang: 
ms.reviewer: kfend
# ms.tgt_pltfrm: 
# ms.custom: 
ms.search.region: India
# ms.search.industry: 
ms.author: epopov
ms.search.validFrom: 2019-08-04
ms.dyn365.ops.version: 10.0.5

---

# Stock transfer orders for India

[!include [banner](../includes/banner.md)]

You can use transfer orders to process inventory transfers between warehouses. In India, if the shipping and receiving branches of the organization have different tax registration numbers, the India Goods and Services Tax (GST) should be calculated and posted for the transfer order. The tax base may be defined as the current cost price of the item being transferred or a special transfer price. The tax amount should be posted as GST payable for the transfer shipment and as GST recoverable upon the transfer receipt. An **Interim transit** account is used as an offset account for the posting and is nullified when the transfer order is fully received. 

The **Stock transfer** functionality that is available for India supports this process.

> [!NOTE]
> Unrealized profit or loss is not posted during the shipment or receipt of a stock transfer line if the unit price differs from the inventory cost price of the item.
> If mutual settlements and profit / loss transactions are required for internal transfers between own company's warehouses, other Dynamics 365 functionalities may be used for this. For example, Purchase orders and Sales orders invoices or Intercompany may be configured for intracompany transfers.
> For more information, see [Set up intercompany trade](../../supply-chain/sales-marketing/intercompany-trade-set-up.md).

## Set up stock transfers

### Enable stock transfer functionality

Enable the following features in the **Feature management** workspace:

- (India) Improvements in unit price and cost price handling in Stock transfer orders
- (Stock transfer for India) Set up the default transfer type and price type for transfer orders created from Master planning
- Enable uniform tax amount and GST transaction ID for both shipment and receipt transaction of a stock transfer order

> [!NOTE]
> Pay your attension that the above features will be turned on by default in one of the future releases.

For more information, see [Feature management overview](../../fin-ops-core/fin-ops/get-started/feature-management/feature-management-overview.md).

You also need to configure the India GST functionality to enable GST calculation for stock transfer orders. For more information, see [India Goods and Services Tax (GST) overview](apac-ind-gst.md).   

> [!NOTE]
> 
> - 
> - Changing the unit of measure on stock a transfer order line is restricted.

### Set up default transfer type and price type for transfer orders

You can define a default transfer order type and a default price type for transfer orders that are created manually. On the **Inventory and warehouse management parameters** page, on the **Transfer orders** tab, select **Stock transfer** in the **Transfer type** field to enable the **Stock transfer** functionality for all newly created transfer orders.  In the **Price type** field, select a default price type for newly created stock transfer orders:

- **Cost price** – The cost price, or the on-hand price, of the item will be used for stock transfer orders.
- **Transfer price** – The transfer price that is set up for the item will be used for stock transfer orders.

> [!NOTE]
> In the case of the **Cost price** price type, the system calculates a running average cost price based on the inventory dimensions specified for an item line. This running average price is stored to the cost price field either on creation of the line, or when it is updated. Thereafter, this saved cost price is never recalculated when shipping and receiving transactions are posted.

### Configure Master planning parameters

You can define a default transfer order type and a default price type for transfer orders that are created when confirming planned orders in the **Master planning** module. On the **Master planning parameters** page, on the **Standard update** tab, select **Transfer type** and **Price type** in the **Firm - Transfer** group.

> [!NOTE]
> These parameters are only available if the "(Stock transfer for India) Set up the default transfer type and price type for transfer orders created from Master planning" feature is enabled in the **Feature management** workspace. Otherwise, default transfer type and price type are defined by corresponding parameters on the **Inventory and warehouse management parameters** page.

### Set up item master parameters

To calculate GST for an item in a stock transfer order, you need to configure certain parameters on the **Released product details** page, such as **HSN codes** and **Tax rate type**. For more information, see [Assign HSN codes and SACs to products](apac-ind-gst-hsn-service-accounting-codes.md#assign-hsn-codes-and-sacs-to-products).

### Configure transfer pricing

You can configure the prices that will be used when processing stock transfers for an item. To do this, open the **Transfer price** page from the **Released product details** page.

### Configure stock transfer posting

You must configure the **Interim transit** account, which is done on the following pages:

- In the **Inventory management** module, on the **Posting** page, on the **Transfer order** tab, specify the **Interim transit** account.
- On the **Tax setup** page, click **Setup**, and on the **Setup** page specify the **Interim transit for stock transfer** account for all GST components that may be created for GST on stock transfer. For more information, see [Define main accounts](apac-ind-gst-map-configuration-tax-types.md#define-main-accounts-1).

> [!NOTE]
> The accounts that are used to post interim transit amounts should have **Interim transit** specified in the **Posting type** field on the **Posting validation** tab of the **Main accounts** page.

You must also set up main accounts to post inventory cost for transfer orders to. Specify the following accounts from the **Inventory** tab on the **Posting** page in **Inventory management**: 

- **Inventory issue**
- **Inventory receipt**
- **Inter-unit payable**
- **Inter-unit receivable**
- **Inventory expenditure, loss**

> [!NOTE]
> The **Inventory expenditure, loss** account is required to post the scrap amount when receiving a transfer order with scrap.

### Configure posting to General ledger based on financial dimension links to sites

In the standard Dynamics 365 functionality, transfer orders are only posted to General ledger if:
1. Dimension link is activated, that is, there is a link between the Site inventory dimension and some financial dimension; and
2. The transfer order movement happens between warehouses that belong to different sites. This condition applies to the From, Transit, and To warehouses. If the From warehouse and Transit warehouse belong to the same site, the transfer order shipment is not posted to General ledger. Similarly, if the Transit warehouse and To warehouse belong to the same site, the transfer order receipt is not posted to General ledger.

If it is required to post stock transfers to General ledger and track inventory in transit warehouses, it is recommended to enable Dimension link and create separate sites for transit warehouses.

For more information, see [Configure and manage financial dimension links to sites](https://docs.microsoft.com/dynamicsax-2012/appuser-itpro/configure-and-manage-financial-dimension-links-to-sites).

> [!NOTE]
> Financial dimensions are derived from products master-data and the above mentioned dimension link. There is no possibility to change financial dimensions in Transfer orders.

## Create and post a stock transfer order

The following example scenario shows frequently performed actions that are associated with transfer orders where the **Transfer type** field is set to **Stock transfer** and **Dimension link** is **enabled**.

1. Go to **Inventory management** > **Outbound orders** > **Transfer order** and create a new transfer order.
1. In the **From warehouse** field, select the supply warehouse that the items are dispatched from.
1. In the **To warehouse** field, select the receiving warehouse that the items are delivered to.

    > [!NOTE]
    > If the From warehouse and Transit warehouse belong to the same site, the transfer order shipment is not posted to General ledger. Similarly, if the Transit warehouse and To warehouse belong to the same site, the transfer order receipt is not posted to General ledger.

1. In the **Transfer type** field, select **Stock transfer** to apply GST to the transfer of items.

    > [!NOTE]
    > If you select **Transfer order** in the **Transfer type** field, the transfer order will be posted based on the standard transfer order process.

1. In the **Price type** field, select a default price type for transfer order lines.
1. On the **Transfer order lines** tab, create a new line and in the **Item number** field, select the item to transfer.
1. In the **Transfer quantity** field, enter the quantity of the items to transfer, and in the **Unit** field, modify the default unit of measurement, if required.

    > [!NOTE]
    > Changing the unit of measure on stock a transfer order line is restricted.

1. In the **Price type** field, select the price type for the transfer order line, from the following options:
    
    - **Cost price** – The cost price, or the on-hand price, of the item is used for the transfer order line.
    - **Transfer price** – The transfer price that is set up for the item is used for the transfer order line.
    
    > [!NOTE]
    > If the **Price type** is set to **Transfer price**, the quantity of the items that are defined for the combination of item and dimension on the **Transfer price** page is displayed in the **Transfer quantity** field, but it can be modified.

1. In the **Unit price** field, enter the cost price or the transfer price for one unit of the item.

	> [!NOTE]
    > - The unit price on a stock transfer order line is recalculated based on the inventory dimensions specified in the line until the first shipment is posted for the line.
	> - Shipment of a stock transfer order line with a unit price of zero (0) can be controlled through the **Prevent shipment with zero unit price** of **Inventory and warehouse management parameters**.

1. Click on the **Tax information** button to check taxes configuration for the transfer order.
    
    - At the line level, under the **Invent transfer order from location** tab, you will find tax information linked to the shipment (from warahouse).
    - At the line level, under the **Invent transfer order to location** tab, you will find tax information linked to the receiving (to warahouse).
    
1. Go to **Inquiries** > **Tax document** and verify that the tax is calculated correctly.
1. Select **Ship** > **Ship transfer order**, and on the **Shipment** page, post the transfer order shipment.

    - Select the **Explode lines** check box.
    - In the **Update** field, select **All**.
    - Select **Setup** > **Tax document**.
	- Check tax settings and amounts, make and apply adjustment if needed.
	- **Close** the **Tax document** form.
	- Click **OK** in the bottom of the **Ship** form to post the operation.	

1. Select **Receive** > **Receive**, and on the **Receive** page, post the transfer order receipt.

    - Select the **Explode lines** check box.
    - In the **Update** field, select **All**.
    - Select **Setup** > **Tax document**.
	- Check tax settings and amounts, make and apply adjustment if needed.
	- **Close** the **Tax document** form.
	- Click **OK** in the bottom of the **Receive** form to post the operation.	

    > [!NOTE]
    > If the "Enable uniform tax amount and GST transaction ID for both shipment and receipt transaction of a stock transfer order" feature is enabled in the **Feature management** workspace, it is only possible to receive a previously posted shipment. You need to select **Shipment** in the **Update** field when posting a receipt and select a previously posted shipment in the **Shipment voucher** field.
    
You can also cancel a previously posted stock transfer order shipment if no receipts have been posted for this order. On the **Transfer orders** page, select **Transfer order** > **Transfer order history**. On the **Transfer order history** page, select a previously posted shipment. Select **Cancel**, and confirm the cancellation of the shipment. The shipment will be canceled, and all inventory movements and GST that was posted for the shipment will be reversed. The "Transfer Order Cancellation" feature in the **Feature management** workspace must be enabled to cancel transfer order shipments.

Below you can find several examples of how stock transfer orders are posted on different conditions.

### Example 1: Standard transfer order posting

Preconditions for the example are as follows:

- Current inventory **cost price** at the moment the shipment is posted is **100 Rs**.
- From warehouse -> Site 1
- Transit warehouse -> Site 2
- To warehouse -> Site 3

#### Transfer order shipment posting:

|      Ledger account name        | Financial dimension linked to site | Debit amount (Rs.) | Credit amount (Rs.) |
|---------------------------------|------------------------------------|--------------------|---------------------|
| Inventory issue                 | Site 1                             |                    |         100         |  
| Inventory inter-unit receivable | Site 1                             |        100         |                     |
| Inventory inter-unit payable    | Site 2                             |                    |         100         | 
| Inventory receipt               | Site 2                             |        100         |                     |

#### Transfer order receipt posting:

|      Ledger account name        | Financial dimension linked to site | Debit amount (Rs.) | Credit amount (Rs.) |
|---------------------------------|------------------------------------|--------------------|---------------------|
| Inventory issue                 | Site 2                             |                    |         100         |  
| Inventory inter-unit receivable | Site 2                             |        100         |                     |
| Inventory inter-unit payable    | Site 3                             |                    |         100         | 
| Inventory receipt               | Site 3                             |        100         |                     |

The **balance on the inventory in transit** can be calculated as **_InventoryIssue-Site2 - InventoryReceipt-Site2_**. It is nullified upon the receipt.

### Example 2: Standard transfer order posting without transit site

Preconditions for the example are as follows:

- Current inventory **cost price** at the moment the shipment is posted is **100 Rs**.
- From warehouse -> Site 1
- Transit warehouse -> Site 1
- To warehouse -> Site 3

##### Transfer order shipment posting:
* no posting to General ledger occurs.

##### Transfer order receipt posting:

|      Ledger account name        | Financial dimension linked to site | Debit amount (Rs.) | Credit amount (Rs.) |
|---------------------------------|------------------------------------|--------------------|---------------------|
| Inventory issue                 | Site 1                             |                    |         100         |  
| Inventory inter-unit receivable | Site 1                             |        100         |                     |
| Inventory inter-unit payable    | Site 3                             |                    |         100         | 
| Inventory receipt               | Site 3                             |        100         |                     |

In this case, **no inventry in transit balance** is tracked.

### Example 3: Stock transfer order that have tax on the transfer price (GST)

Preconditions for the example are as follows:

- Current inventory **cost price** at the moment the shipment is posted is **100 Rs**.
- **Unit price** specified in the line is **120 Rs**. It can be the inventory cost price at the moment the line was created or updated, or it can be the transfer price of the item from the dictionaty.
- Taxable value (base amount): 120.00
- **GST** rate: **10%**"
- From warehouse -> Site 1
- Transit warehouse -> Site 2
- To warehouse -> Site 3

#### Transfer order shipment posting:

|      Ledger account name        | Financial dimension linked to site | Debit amount (Rs.) | Credit amount (Rs.) |
|---------------------------------|------------------------------------|--------------------|---------------------|
| Inventory issue                 | Site 1                             |                    |         100         |  
| Inventory inter-unit receivable | Site 1                             |        100         |                     |
| Inventory inter-unit payable    | Site 2                             |                    |         100         | 
| Inventory receipt               | Site 2                             |        100         |                     |
| GST payable                     | Site 1                             |                    |          12         | 
| Interim account                 | Site 1                             |         12         |                     |

#### Transfer order receipt posting:

|      Ledger account name        | Financial dimension linked to site | Debit amount (Rs.) | Credit amount (Rs.) |
|---------------------------------|------------------------------------|--------------------|---------------------|
| Inventory issue                 | Site 2                             |                    |         100         |  
| Inventory inter-unit receivable | Site 2                             |        100         |                     |
| Inventory inter-unit payable    | Site 3                             |                    |         100         | 
| Inventory receipt               | Site 3                             |        100         |                     |
| GST receivable                  | Site 3                             |                    |          12         |  
| Interim account                 | Site 3                             |         12         |                     |

The **balance on the inventory in transit** can be calculated as **_InventoryIssue-Site2 - InventoryReceipt-Site2_**. It is nullified upon the receipt.

> [!NOTE]
>
> - **Inventory issue** and **Inventory receipt** are most likely one and the same balance account, such as "Finished goods".
> - The above settings for Dimension links and sites (that is, a separate site for the transit warehouse) are recommended if it is needed to track the in-transit inventory in General ledger.
  
### Example 4: Stock transfer order with standard cost

Preconditions for the example are as follows:

- Create an item with the **Standard cost** inventory model
- Create and active different item prices for the item with Costing type = Standard cost and Price type = Cost:
	- price = 115 for Site 1 
	- price = 112 for Site 2 
	- price = 111 for Site 3
- Inventory **cost price** is defined by the stadard cost on the "From warehouse" site and is equal to  **115 Rs** in our case.
- From warehouse -> Site 1
- Transit warehouse -> Site 2
- To warehouse -> Site 3

#### Transfer order shipment posting:

|      Ledger account name        | Financial dimension linked to site | Debit amount (Rs.) | Credit amount (Rs.) |
|---------------------------------|------------------------------------|--------------------|---------------------|
| Inventory issue                 | Site 1                             |                    |         115         |  
| Inventory inter-unit receivable | Site 1                             |        115         |                     |
| Inventory inter-unit payable    | Site 2                             |                    |         115         | 
| Inventory receipt               | Site 2                             |        112         |                     |
| Cost change variance            | Site 2                             |          3         |                     | 

#### Transfer order receipt posting:

|      Ledger account name        | Financial dimension linked to site | Debit amount (Rs.) | Credit amount (Rs.) |
|---------------------------------|------------------------------------|--------------------|---------------------|
| Inventory issue                 | Site 2                             |                    |         112         |  
| Inventory inter-unit receivable | Site 2                             |        112         |                     |
| Inventory inter-unit payable    | Site 3                             |                    |         112         | 
| Inventory receipt               | Site 3                             |        111         |                     |
| Cost change variance            | Site 3                             |          1         |                     | 

The **balance on the inventory in transit** can be calculated as **_InventoryIssue-Site2 - InventoryReceipt-Site2_**. It is nullified upon the receipt.

  
[!INCLUDE[footer-include](../../includes/footer-banner.md)]
