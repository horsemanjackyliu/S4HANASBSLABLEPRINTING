---
noteId: "3ce6d1d00d4111ef88964ff9e42a747e"
tags: []

---

### Prerequisites:

- [Set Up SAP Business Application Studio for Development in the same BTP subaccount](https://developers.sap.com/tutorials/appstudio-devspace-fiori-create.html)
- The following roles has been assigned to your user account in BTP subaccount.
![alt text](image.png)
- [Create a Dev Space for SAP Fiori Apps](https://developers.sap.com/tutorials/appstudio-devspace-fiori-create.html)


### Step 1, Login into cloud foundry in BAS.

![alt text](image-6.png)
get API endpoint of cloud foundry in BTP Subaccount which include the BTP ABAP Environment.
![alt text](image-5.png)
![alt text](image-7.png)
![alt text](image-8.png)
![alt text](image-9.png)
![alt text](image-10.png)
![alt text](image-11.png)

### Step 2, Create new fiori project with from template in BAS.
![alt text](image-1.png)
![alt text](image-2.png)
![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-12.png)
![alt text](image-13.png)
![alt text](image-14.png)
![alt text](image-15.png)
![alt text](image-16.png)

### Step 3, Adjust the fiori project to add controller extension in BAS.

![alt text](image-17.png)
![alt text](image-18.png)
![alt text](image-19.png)
![alt text](image-20.png)

![alt text](image-21.png)
Input the controller name  **DnitemController**
![alt text](image-22.png)

![alt text](image-23.png)

Update the controller's code as the following:


```
sap.ui.define(['sap/ui/core/mvc/ControllerExtension','sap/base/security/URLWhitelist','sap/ui/model/json/JSONModel'], function (ControllerExtension,URLWhitelist,JSONModel) {
	'use strict';

	return ControllerExtension.extend('zlabelprint.ext.controller.DnitemController', {
		// this section allows to extend lifecycle hooks or hooks provided by Fiori elements
		render:function(oEvent){

			let showPdf = null;
			var oModel = this.base.getExtensionAPI().getModel();
		   //  var sPath = oModel.getBindings()[0].getContext().sPath;
		   var material =  oModel.getBindings().filter(bn=>{ return bn.sPath=='material' })[0].vValue
		   var packageNo =  oModel.getBindings().filter(bn=>{ return bn.sPath=='total_number_of_package' })[0].vValue
		   var dn =  oModel.getBindings().filter(bn=>{ return bn.sPath=='delivery_document' })[0].vValue
		   var dnItem =  oModel.getBindings().filter(bn=>{ return bn.sPath=='delivery_document_item' })[0].vValue
		   var quantity =  oModel.getBindings().filter(bn=>{ return bn.sPath=='actual_delivery_quantity' })[0].vValue
var filename = dn+dnItem ;

			const sFunctionname = "com.sap.gateway.srvd.zsrv_dn.v0001.render";
			var sPath = this.getView().getBindingContext().getPath();
			const oFunction = oModel.bindContext(`${sPath}/${sFunctionname}(...)`);
			oFunction.setParameter("materialNo", material);
			oFunction.setParameter("quantity", quantity.toString());
			oFunction.setParameter("packageNo", packageNo);
		    oFunction.execute().then(function(){
				const oContext = oFunction.getBoundContext();
				var stream = oContext.getProperty('stream');
				stream = stream.replaceAll('_','/').replaceAll('-','+');
				const deccont = atob(stream);			
				const byteNumbers = new Array(deccont.length);
				for (let i = 0; i < deccont.length; i++) {
					byteNumbers[i] = deccont.charCodeAt(i);
				}
				const byteArray = new Uint8Array(byteNumbers);
				var blob = new Blob([byteArray],{type: "application/pdf"});
				const pdfurl = URL.createObjectURL(blob);
				let oPdfmodel = new JSONModel({
					Source: pdfurl,
					Title: filename,
					Height: "1000px"
				});
				URLWhitelist.add("blob");
				this.getView().setModel(oPdfmodel,"pdf");
				this.getView().getModel('pdfview').setData({"Viewshow":true});


			}.bind(this)).catch(err=>{
				console.log('function err happened');
				console.log(err);
			});




		},
		override: {
			/**
             * Called when a controller is instantiated and its View controls (if available) are already created.
             * Can be used to modify the View before it is displayed, to bind event handlers and do other one-time initialization.
             * @memberOf zlabelprint.ext.controller.DnitemController
             */
			onInit: function () {
				// you can access the Fiori elements extensionAPI via this.base.getExtensionAPI
				var oModel = this.base.getExtensionAPI().getModel();
				let oPdfview = new JSONModel({
					Viewshow: false
				});

				this.getView().setModel(oPdfview,"pdfview");
			},
			routing:{
				onAfterRendering: function(){
					let oPdfview = new JSONModel({
						Viewshow: false
					});
	
					this.getView().setModel(oPdfview,"pdfview");
	
				}
			}
		}
	});
});
```

### Step 4, Adjust the fiori project to add customer fragment and Custom Action in BAS.
![alt text](image-24.png)
![alt text](image-25.png)
![alt text](image-26.png)
![alt text](image-27.png)

Update the fragment code as the following:

```
<core:FragmentDefinition xmlns:core="sap.ui.core" xmlns="sap.m" xmlns:macros="sap.fe.macros">
<ScrollContainer id="_IDGenScrollContainer1"
		height="100%"
		width="100%"
		horizontal="true"
		vertical="true" visible="{pdfview>/Viewshow}">
		<FlexBox id="_IDGenFlexBox1" direction="Column" renderType="Div" class="sapUiSmallMargin">
			<PDFViewer id="_IDGenPDFViewer1" source="{pdf>/Source}" isTrustedSource="true" title="{pdf>/Title}" height="{pdf>/Height}" >
				<layoutData>
					<FlexItemData id="_IDGenFlexItemData1" growFactor="1" />
				</layoutData>
			</PDFViewer>
		</FlexBox>
	</ScrollContainer>
</core:FragmentDefinition>

```
![alt text](image-29.png)
![alt text](image-30.png)

### Step 5, Adjust the fiori project to Flexible Column Layout in BAS.
![alt text](image-28.png)

### Step 6, Preview the applicaton in BAS.
![alt text](image-32.png)
![alt text](image-33.png)
![alt text](image-34.png)
![alt text](image-35.png)
![alt text](image-36.png)
![alt text](image-37.png)











