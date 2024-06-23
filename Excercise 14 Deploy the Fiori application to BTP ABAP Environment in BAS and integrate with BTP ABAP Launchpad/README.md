
### Step 1,Deploy the fiori application to BTP ABAP Environment in BAS
![alt text](image.png)
![alt text](image-1.png)
### Step 2,Check the deployed fiori application in Eclipse
![alt text](image-3.png)
### Step 3,Create IAM App
In Eclipse right-click your package ZZLABELPRINT and select New > Other Repository Object.
![alt text](image-10.png)
![alt text](image-4.png)
![alt text](image-5.png)

- Name: ZLABELPRINTING_IAM_APP
- Description: Label Printing IAM APP 
- Click Next.
![alt text](image-6.png)
![alt text](image-7.png)
![alt text](image-8.png)
![alt text](image-9.png)

### Step 4: Create business catalog

In Eclipse right-click your package ZZLABELPRINT and select New > Other Repository Object.

![alt text](image-10.png)
![alt text](image-11.png)

![alt text](image-12.png)

- Name: ZLABELPRINT_BC2
- Description: Label Printing Businessf Catalog
![alt text](image-13.png)
![alt text](image-14.png)
![alt text](image-15.png)
![alt text](image-16.png)
![alt text](image-17.png)
![alt text](image-18.png)
![alt text](image-25.png)


### Step 5: Create business role in BTP ABAP Environment.
![alt text](image-19.png)
![alt text](image-20.png)
![alt text](image-21.png)
![alt text](image-22.png)
![alt text](image-23.png)

- Business Role ID: BR_Z_LABELPRINT
- Business Role Description: Label Printing Business Role

![alt text](image-24.png)
![alt text](image-26.png)
![alt text](image-27.png)
![alt text](image-28.png)
![alt text](image-29.png)


### Step 6:Create transport request or use default transport request

Log in to your system and select the Export Customizing Transports tile
![alt text](image-32.png)
![alt text](image-30.png)
![alt text](image-31.png)
![alt text](image-33.png)


Create new transport request:

- Description: Transport_Request_LabelPrint
- Type: Customizing Request
![alt text](image-34.png)
![alt text](image-35.png)

If the Transport Category is not default, click **change Category** to change it to default .

![alt text](image-36.png)

## Step 7: Create Launchpad Space.
![alt text](image-37.png)
Select the Manage Launchpad Spaces tile.
![alt text](image-38.png)
![alt text](image-39.png)
Create new space and page:

- Space ID: ZLABELPRINT_SPACE 
- Space description: Space for label printing
- Space title: Space for label printing
- Page ID: ZLABELPRINT_PAGE
- Page description: Page for label printing
- Page title:  Page for label printing
- Transport: H02K900059 (user your own cts)
![alt text](image-40.png)
![alt text](image-41.png)
![alt text](image-42.png)
![alt text](image-43.png)
![alt text](image-44.png)
![alt text](image-45.png)
![alt text](image-46.png)
![alt text](image-47.png)
![alt text](image-48.png)
![alt text](image-49.png)

## Step 8: Run the application
![alt text](image-50.png)
![alt text](image-51.png)
![alt text](image-52.png)
![alt text](image-53.png)







