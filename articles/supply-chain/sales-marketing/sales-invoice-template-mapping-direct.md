---
# required metadata

title: Synchronize sales invoice headers and lines directly from Finance and Operations to Sales
description: This topic discusses the templates and underlying tasks that are used to synchronize sales invoice headers and lines directly from Microsoft Dynamics 365 for Finance and Operations, Enterprise edition, to Microsoft Dynamics 365 for Sales. 
author: ChristianRytt
manager: AnnBe
ms.date: 10/26/2017
ms.topic: article
ms.prod: 
ms.service: dynamics-ax-applications
ms.technology: 

# optional metadata

ms.search.form: 
# ROBOTS: 
audience: Application User, IT Pro
# ms.devlang: 
ms.reviewer: yuyus
ms.search.scope: Core, Operations, UnifiedOperations
# ms.tgt_pltfrm: 
ms.custom: 
ms.assetid: 
ms.search.region: global
ms.search.industry: 
ms.author: crytt
ms.dyn365.ops.intro: July 2017 update 
ms.search.validFrom: 2017-07-8

---

# Synchronize sales invoice headers and lines directly from Finance and Operations to Sales

[!include[banner](../includes/banner.md)]

This topic discusses the templates and underlying tasks that are used to synchronize sales invoice headers and lines directly from Microsoft Dynamics 365 for Finance and Operations, Enterprise edition, to Microsoft Dynamics 365 for Sales.

## Data flow in Prospect to cash

The Prospect to cash solution uses the Data integration feature to synchronize data across instances of Finance and Operations and Sales. The Prospect to cash templates that are available with the Data integration feature enable the flow of data about accounts, contacts, products, sales quotations, sales orders, and sales invoices between Finance and Operations and Sales. The following illustration shows how the data is synchronized between Finance and Operations and Sales.

[![Data flow in Prospect to cash](./media/prospect-to-cash-data-flow.png)](./media/prospect-to-cash-data-flow.png)

## Templates and tasks

To access the available templates, open [PowerApps Admin Center](https://preview.admin.powerapps.com/dataintegration). Select **Projects**, and then, in the upper-right corner, select **New project** to select public templates.

The following template and underlying tasks are used to synchronize sales invoice headers and lines from Finance and Operations to Sales:

- **Name of the template in Data integration:** Sales Invoices (Fin and Ops to Sales) - Direct
- **Names of the tasks in the Data integration project:**

    - SalesInvoiceHeader
    - SalesInvoiceLine

The following synchronization tasks are required before synchronization of sales invoice headers and lines can occur:

- Products (Fin and Ops to Sales) - Direct
- Accounts (Sales to Fin and Ops) - Direct (if used)
- Contacts (Sales to Fin and Ops) - Direct (if used)
- Sales order header and lines (Fin and Ops to Sales) - Direct

## Entity set

| Finance and Operations                               | Sales          |
|------------------------------------------------------|----------------|
| Externally maintained customer sales invoice headers | Invoices       |
| Externally maintained customer sales invoice lines   | InvoiceDetails |

## Entity flow

Sales invoices are created in Finance and Operations and synchronized to Sales.

> [!NOTE]
> Currently, tax that is related to charges on the sales invoice header isn't included in the synchronization from Finance and Operations to Sales. Sales doesn't support tax information at the header level. However, tax that is related to charges at the line level is included in the synchronization.

## Prospect to cash solution for Sales

- An **Invoice number** field has been added to the **Invoice** entity and appears on the page.
- The **Create invoice** button on the **Sales order** page is hidden, because invoices will be created in Finance and Operations and synchronized to Sales. The **Invoice** page can't be edited, because invoices will be synchronized from Finance and Operations.
- The **Sales order status** value is automatically changed to **Invoiced** when the related invoice from Finance and Operations has been synchronized to Sales. Additionally, the owner of the sales order that the invoice was created from is assigned as the owner of the invoice. Therefore, the owner of the sales order can view the invoice.

## Preconditions and mapping setup

Before you synchronize sales invoices, it's important that you update the following settings in the systems.

### Setup in Sales

Go to **Settings** > **Administration** > **System settings** > **Sales**, and make sure that the following settings are used:

- The **Use system prizing calculation system** option is set to **Yes**.
- The **Discount calculation method** field is set to **Line item**.

### Setup in the Data integration project

#### SalesInvoiceHeader task

- Make sure that the required mapping exists for **InvoiceCountryRegionId** to **BillingAddress\_Country**.

    The template value is a value map where several countries or regions are mapped.

- A price list is required in order to create invoices in Sales. Update the value map for **pricelevelid.name \[Price list name\]** to the price list that is used in Sales per currency. You can use the default price list for a single currency. Alternatively, if you have price lists in multiple currencies, you can use a value map.

    The template value for **pricelevelid.name \[Price list name\]** is a value map that is based on currency with USD = CRM Service USA (sample).  
    
#### SalesInvoiceLine task

- Make sure that the required mapping exists for **Unit of measure**.
- Make sure that the required value map for **SalesUnitSymbol** in Finance and Operations exists.

    A template value that has a value map is defined for **SalesUnitSymbol** to **Quantity\_UOM**.

## Template mapping in Data integration

> [!NOTE]
> The **Payment terms**, **Freight terms**, **Delivery terms**, **Shipping method**, and **Delivery mode** fields aren't included in the default mappings. To map these fields, you must set up a value mapping that is specific to the data in the organizations that the entity is synchronized between.

The following illustrations show an example of a template mapping in Data integration. 

> [!NOTE]
> The mapping shows which field information will be synchronized from Sales to Finance and Operations.

### SalesInvoiceHeader

![Template mapping in Data integration](./media/sales-invoice-direct-template-mapping-data-integrator-1.png)

### SalesInvoiceLine

![Template mapping in Data integration](./media/sales-invoice-direct-template-mapping-data-integrator-2.png)



## Related topics

[Prospect to cash](prospect-to-cash.md)

[Synchronize accounts directly from Sales to customers in Finance and Operations](accounts-template-mapping-direct.md)

[Synchronize products directly from Finance and Operations to products in Sales](products-template-mapping-direct.md)

[Synchronize contacts directly from Sales to contacts or customers in Finance and Operations](contacts-template-mapping-direct.md)

[Synchronize sales order headers and lines directly from Finance and Operations to Sales](sales-order-template-mapping-direct.md)





