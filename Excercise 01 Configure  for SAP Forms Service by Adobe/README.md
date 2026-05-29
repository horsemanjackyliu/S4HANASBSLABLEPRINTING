# Exercise 01: Configure SAP Forms Service by Adobe

In this exercise you will provision the **SAP Forms Service by Adobe** in your BTP subaccount and upload the label print template that the solution uses to generate PDFs. This service is the rendering engine called in Exercise 11 when `cl_fp_ads_util=>render_4_pq` converts XML delivery data into a printable label.

---

## Steps Overview

Complete steps 1–5 of the following SAP Community blog post, which covers the full configuration of SAP Forms Service by Adobe in a BTP Cloud Foundry subaccount:

**[Configure the SAP BTP Cloud Foundry Environment Sub-account with SAP Forms Service by Adobe and Test with Postman](https://community.sap.com/t5/technology-blogs-by-sap/configure-the-sap-btp-cloud-foundry-environment-subaccount-with-sap-forms/ba-p/13536407)**

The steps in the blog cover the following tasks:

| Step | Task |
|------|------|
| 1 | Create a new subscription for **Forms Service by Adobe** (plan: `default`) |
| 2 | Assign the following roles to your admin user: |
| | `ADSAdmin` — access to the Forms Service Config UI (fonts, XDC, XCI settings) |
| | `TemplateStoreAdmin` — access to the Template Store UI for managing form templates |
| 3 | Create a new service instance for **Forms Service by Adobe API** (plan: `standard`) |
| 4 | Create a service key for the instance |
| 5 | Upload the label print template into the Template Store |

---

## What to Note for Later Exercises

The service key created in Step 4 contains the credentials you will need in **Exercise 02** to configure the Communication System in your BTP ABAP Environment. Keep a copy of it available.

The template uploaded in Step 5 must use:

| Property | Value |
|----------|-------|
| Form name | `labelprint` |
| Template name | `PrintLabel` |

These names are hardcoded in the `render` method of the behavior implementation class `ZBP_OBJ_DN` (Exercise 11) and must match exactly.

---

## Result

Once this exercise is complete, your BTP subaccount has a running Forms Service by Adobe instance with a `labelprint` form and `PrintLabel` template ready to receive rendering requests from the BTP ABAP Environment.
