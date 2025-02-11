1. From the Navigation Pane on the left, select **Connectivity >> Destinations**.  Click **Create Destination**.
![create_destination](1.jpg)

2. Specify the information below to create the destination and click **Save**:
  * Name: **LPS_SFSF_dt**
  * Type: **HTTP**
  * URL: <-- https://yourSFSFtenantAPIURL/rest/servicesfoundation/sfcdmcontentservice/v1/SFCDMContent  (see preparation section to find the API URL for your SuccessFactors instance-->
  * Proxy Type: **Internet**
  * Authentication: **BasicAuthentication**
  * User: <-- SAP SuccessFactors username with oData API access and company ID in the format of **username@COMPANYID** (see preparation section for more information on how to create this API User) -->
  * Password: <-- Password for the user above -->
  * Use default JDK truststore: **checked**
  * **New Property** >> **HTML5.DynamicDestination**: **true**</br>
   ![create_destination](3.jpg)

  **NOTE**: Do not forget to add **/rest/servicesfoundation/sfcdmcontentservice/v1/SFCDMContent** to the end of the URL field.
  
4. Click the pencil icon to edit the **LPS_SFSF_rt** destination that is automatically created by the booster you ran earlier.</br>
![create_destination](4.jpg)

5. Click **New Property** and type **sap-start**.  Set the value of of the propery to **true** and **Save** the destination.</br>
![create_destination](5.jpg)</br>
**Important**: You may also need to the update the URL parameter in this destination in certain scnearios.  For customers whose SuccessFactors tenants are migrated to **Common Super Domain(CSD)** or access SuccessFactors through a **reverse proxy** need to update the URL parameter to match to their CSD or reverse proxy URL.  
**Note**: **sap-start** must be all in lower case.

