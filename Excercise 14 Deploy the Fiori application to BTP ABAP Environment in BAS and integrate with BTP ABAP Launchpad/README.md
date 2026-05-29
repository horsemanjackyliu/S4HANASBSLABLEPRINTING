# Exercise 14: Deploy the Fiori Application and Integrate with the BTP ABAP Launchpad

In this exercise you will deploy the `zlabelprint` Fiori Elements application to the BTP ABAP Environment and make it accessible to business users through the Fiori Launchpad. By the end, users can open the application from a dedicated Launchpad tile, browse outbound deliveries, and trigger label printing without ever leaving the browser.

The exercise has eight parts:

| Part | What you do |
|------|-------------|
| 1 | Deploy the Fiori app from BAS to the BTP ABAP Environment |
| 2 | Verify the deployed app in Eclipse ADT |
| 3 | Create an IAM App and publish it locally |
| 4 | Create a Business Catalog and assign the IAM App |
| 5 | Create a Business Role and grant access |
| 6 | Create a Customizing Transport Request |
| 7 | Create a Launchpad Space and Page, then add the app tile |
| 8 | Run the application from the Launchpad |

---

## Part 1: Deploy the Fiori Application from BAS

### Step 1: Open Application Info and Start Deployment

In BAS, open the `zlabelprint` project. From the **Storyboard**, hover over the `zlabelprint` card and click **Open Application Info**.

In the **Application Info** panel, click **Deploy Application**. BAS first runs a test deployment to validate the configuration.

![Application Info — Test deployment](img/image.png)

When the test deployment completes successfully, click **Deploy** to push the application to the BTP ABAP Environment. BAS uploads the app to the SAPUI5 ABAP Repository `ZLABELPRINT` that was configured in Exercise 13.

![Deployment confirmation](img/image-1.png)

> If prompted for credentials, enter your BTP ABAP Environment user name and password.

---

## Part 2: Verify the Deployed App in Eclipse ADT

### Step 2: Check the BSP Application and App Descriptor in Eclipse

After deployment, switch to Eclipse ADT and open the **Project Explorer**. Expand your package `ZLABELPRINT` and navigate to **BSP Applications**. You should see the BSP application `ZLABELPRINT` listed there.

Also verify that the **Fiori Launchpad App Descriptor Item** `ZLABELPRINT_USR` has been created under **Fiori Launchpad App Descriptor Items**. This descriptor item links the deployed app to the Launchpad.

![Eclipse ADT — BSP Application ZLABELPRINT and App Descriptor ZLABELPRINT_USR](img/image-3.png)

> If either object is missing, re-run the deployment from BAS and check for errors in the deployment log.

---

## Part 3: Create an IAM App in Eclipse ADT

An **IAM App** (Identity and Access Management App) registers the Fiori application and its required OData service as a single security object. You will later assign this IAM App to a Business Catalog so that it can be included in a Business Role.

### Step 3: Open the New Repository Object Wizard

In the **Project Explorer**, right-click the package `ZLABELPRINT` and choose **New → Other ABAP Repository Objects**.

![Right-click ZLABELPRINT → New → Other ABAP Repository Objects](img/image-10.png)

---

### Step 4: Select the IAM App Object Type

In the search box, type `IAM` and select **IAM App** from the results. Click **Next**.

![Select IAM App object type](img/image-4.png)

---

### Step 5: Enter the IAM App Details

Fill in the fields as follows and click **Next**.

![New IAM App wizard — ZLABELPRINTING_IAM_APP](img/image-5.png)

| Field | Value |
|-------|-------|
| Package | `ZLABELPRINT` |
| Name | `ZLABELPRINTING_IAM_APP` |
| Description | `Label Printing IAM APP` |
| App Type | `External App` |

> When you select **External App**, the system automatically appends `_EXT` to the name. The resulting IAM App ID will be `ZLABELPRINTING_IAM_APP_EXT`.

---

### Step 6: Assign the Transport Request

Select the `ZLABELPRINT_PACKAGE` transport request and click **Finish**.

![Select ZLABELPRINT_PACKAGE transport request](img/image-6.png)

---

### Step 7: Assign the Fiori Launchpad App Descriptor Item

Eclipse opens the IAM App editor. In the **Fiori Launchpad App Descr Items** tab, click **+** and add the app descriptor item `ZLABELPRINT_UI5R`.

![IAM App editor — assign Fiori Launchpad App Descr Item ZLABELPRINT_UI5R](img/image-7.png)

---

### Step 8: Add the Required OData Service

Switch to the **Services** tab. Click **+** and add the OData V4 service binding:

![IAM App editor — add OData V4 service ZSRVBND_DN](img/image-8.png)

| Field | Value |
|-------|-------|
| Service Type | `OData V4` |
| Service Name | `ZSRVBND_DN` |

---

### Step 9: Publish the IAM App Locally

Click **Publish Locally** to make the IAM App available to the Business Catalog editor in the next step.

![IAM App — click Publish Locally](img/image-9.png)

> Publishing locally does not deploy to production. It makes the IAM App visible within ADT for assignment to a Business Catalog.

---

## Part 4: Create a Business Catalog in Eclipse ADT

A **Business Catalog** groups one or more IAM Apps into a logical unit that can be assigned to a Business Role. You will create a catalog for the label printing application.

### Step 10: Open the New Repository Object Wizard

In the **Project Explorer**, right-click the package `ZLABELPRINT` and choose **New → Other ABAP Repository Objects** (same starting point as Step 3).

![Right-click ZLABELPRINT → New → Other ABAP Repository Objects](img/image-10.png)

---

### Step 11: Select the Business Catalog Object Type

In the search box, type `Business Catalog` and select **Business Catalog** from the results. Click **Next**.

![Select Business Catalog object type](img/image-11.png)

---

### Step 12: Enter the Business Catalog Details

Fill in the fields as follows and click **Next**.

![New Business Catalog wizard — ZLABELPRINT_BC](img/image-12.png)

| Field | Value |
|-------|-------|
| Package | `ZLABELPRINT` |
| Name | `ZLABELPRINT_BC` |
| Description | `Label Printing Business Catalog` |

---

### Step 13: Assign the Transport Request

Select the `ZLABELPRINT_PACKAGE` transport request and click **Finish**.

![Select transport request](img/image-13.png)

---

### Step 14: Assign the IAM App to the Catalog

Eclipse opens the Business Catalog editor. In the **Apps** tab, click **+** to add the IAM App.

![Business Catalog editor — Apps tab, click +](img/image-14.png)

In the dialog, enter the IAM App name:

| Field | Value |
|-------|-------|
| IAM App | `ZLABELPRINTING_IAM_APP_EXT` |
| Assignment ID | `ZLABELPRINT_BC_0001` (auto-generated or enter manually) |

![Assign IAM App ZLABELPRINTING_IAM_APP_EXT](img/image-15.png)

---

### Step 15: Publish the Business Catalog Locally

Click **Publish Locally** to make the catalog available for assignment to a Business Role.

![Business Catalog — click Publish Locally](img/image-16.png)

After publishing, the **Apps** tab shows the assignment with status **Published**.

![Business Catalog — published assignment](img/image-17.png)

---

## Part 5: Create a Business Role in the BTP ABAP Environment

A **Business Role** grants users access to the apps in a Business Catalog. You will create a role for the label printing application and assign it to the relevant users.

### Step 16: Open the Maintain Business Roles App

In the BTP ABAP Environment Fiori Launchpad, search for `Business Roles` and open **Maintain Business Roles**.

![Search for Maintain Business Roles](img/image-19.png)

---

### Step 17: Create a New Business Role

On the **Maintain Business Roles** list page, click **New**.

![Maintain Business Roles — click New](img/image-20.png)

Fill in the role details:

![New Business Role — BR_Z_LABELPRINT](img/image-21.png)

| Field | Value |
|-------|-------|
| Business Role ID | `BR_Z_LABELPRINT` |
| Business Role Description | `Label Printing Business Role` |

---

### Step 18: Assign the Business Catalog

In the **Assigned Business Catalogs** section, click **Add** and select `ZLABELPRINT_BC`.

![Assign Business Catalog ZLABELPRINT_BC](img/image-22.png)

---

### Step 19: Set Access Categories to Unrestricted

Switch to the **Access Categories** tab. Set all access categories to **Unrestricted** to allow full access to the application.

![Access Categories — set to Unrestricted](img/image-23.png)

---

### Step 20: Assign Users to the Role

Switch to the **Assigned Users** tab and click **Add**. Search for the user(s) who need access to the label printing application and add them.

![Assign users to BR_Z_LABELPRINT](img/image-24.png)

Click **Save** to create the Business Role.

![Business Role saved](img/image-26.png)

---

## Part 6: Create a Customizing Transport Request

Launchpad Spaces and Pages are customizing objects that must be transported between systems. Before creating the Launchpad Space, you need a **Customizing Transport Request** to record these customizing changes.

### Step 21: Open the Export Customizing Transports App

In the BTP ABAP Environment Fiori Launchpad, search for `Export Customizing` and open **Export Customizing Transports**.

![Search for Export Customizing Transports](img/image-32.png)

---

### Step 22: Create a New Transport Request

On the **Export Customizing Transports** list page, click **Create**.

![Export Customizing Transports — click Create](img/image-30.png)

Fill in the transport request details:

![New transport request dialog](img/image-31.png)

| Field | Value |
|-------|-------|
| Description | `Transport_Request_LabelPrint` |
| Type | `Customizing Request` |

Click **Create**. The system generates a transport request number (e.g. `H02K900059`). Note this number — you will use it when creating the Launchpad Space.

---

### Step 23: Verify the Transport Category

Open the newly created transport request and check the **Category** field. If it is not set to **Default**, click **Change Category** and set it to **Default**.

![Transport request — change Category to Default](img/image-33.png)

![Transport request confirmed with Default category](img/image-34.png)

> The **Default** category is required for Launchpad customizing objects to be recorded correctly.

---

## Part 7: Create a Launchpad Space and Page

A **Launchpad Space** is a named area on the Fiori Launchpad home page. Each space contains one or more **Pages**, and each page contains tiles that open applications. You will create a dedicated space for the label printing application.

### Step 24: Open the Manage Launchpad Spaces App

In the BTP ABAP Environment Fiori Launchpad, navigate to **Administration → Launchpad** and open **Manage Launchpad Spaces**.

![Administration → Launchpad → Manage Launchpad Spaces](img/image-37.png)

---

### Step 25: Create a New Space

On the **Manage Launchpad Spaces** list page, click **Create**.

![Manage Launchpad Spaces — click Create](img/image-38.png)

Fill in the space details. When prompted for a transport request, select `H02K900059` (the request you created in Part 6).

![New Launchpad Space dialog](img/image-39.png)

| Field | Value |
|-------|-------|
| Space ID | `ZLABELPRINT_SPACE` |
| Space Description | `Space for label printing` |
| Space Title | `Space for label printing` |
| Transport | `H02K900059` (use your own transport request number) |

---

### Step 26: Create a Page within the Space

After the space is created, click **Create Page** (or navigate to the **Pages** tab and click **+**) to add a page to the space.

![Create Page within the space](img/image-40.png)

| Field | Value |
|-------|-------|
| Page ID | `ZLABELPRINT_PAGE` |
| Page Description | `Page for label printing` |
| Page Title | `Page for label printing` |
| Transport | `H02K900059` |

![New page details](img/image-41.png)

---

### Step 27: Assign the Business Role to the Space

In the space editor, go to the **Assigned Roles** section and click **+**. Add the business role `BR_Z_LABELPRINT` so that users with this role can see the space on their Launchpad.

![Assign business role BR_Z_LABELPRINT to the space](img/image-42.png)

---

### Step 28: Add the Application Tile to the Page

Open the page `ZLABELPRINT_PAGE` and click **Add Tile** (or **+**). Search for the tile from the catalog `ZLABELPRINT_BC`.

![Add tile to ZLABELPRINT_PAGE](img/image-43.png)

Select the tile **Outbound Delivery Application** and confirm.

![Select Outbound Delivery Application tile](img/image-44.png)

The tile appears on the page.

![Page with Outbound Delivery Application tile added](img/image-45.png)

Save the page and space.

![Launchpad Space saved](img/image-46.png)

---

## Part 8: Run the Application from the Launchpad

### Step 29: Open the Launchpad and Find the Space

Log in to the BTP ABAP Environment Fiori Launchpad as a user assigned the `BR_Z_LABELPRINT` role. The **Space for label printing** tab appears on the Launchpad home page.

![Launchpad home — Space for label printing tab](img/image-50.png)

---

### Step 30: Open the Application

Click the **Space for label printing** tab to open it. The page shows the **Outbound Delivery Application** tile. Click the tile to launch the application.

![Outbound Delivery Application tile on the Launchpad](img/image-51.png)

---

### Step 31: Browse Deliveries and Print a Label

The app opens with the **List Report** showing outbound deliveries. Click **Go** to load the delivery list.

![List Report — outbound deliveries](img/image-52.png)

Select a delivery to open its Object Page, then click on a delivery item. On the item Object Page, click **Render PDF** to trigger the label printing action. The rendered PDF label appears inline in the **PDFRenderView** section.

![Delivery item Object Page — PDF label rendered](img/image-53.png)

---

## Result

The `zlabelprint` Fiori Elements application is now fully deployed and accessible to business users. The complete solution is in place:

- The app is deployed to the BTP ABAP SAPUI5 repository `ZLABELPRINT`
- The IAM App `ZLABELPRINTING_IAM_APP_EXT` registers the application and its OData service
- The Business Catalog `ZLABELPRINT_BC` groups the IAM App for role assignment
- The Business Role `BR_Z_LABELPRINT` grants users access to the application
- The Launchpad Space `ZLABELPRINT_SPACE` and Page `ZLABELPRINT_PAGE` surface the application tile
- Users can browse outbound deliveries and print shipping labels directly from the Fiori Launchpad
