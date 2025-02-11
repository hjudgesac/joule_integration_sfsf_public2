## **Configure SuccessFactors as Source System in Identity Provisioning**

1. Download [**IdentityProvisioningFiles.zip**](https://github.com/hjudgesac/joule_integration_sfsf_public/raw/main/configure_identity_provisioning/files/IdentityProvisioningFiles.zip) that contains the pre-defined source and target systems required to setup this configuration.</br>
**Note**: A file should be automatically downloaded into your downloads folder.
2. Extract the zip file into a folder of your choice.  Confirm that the following 2 files are visible in the extracted folder:
   * **SuccessFactors-SF-Company-ID-Joule.json**
   * **WorkZone_Target_ForJoule.json**</br>
![configure_ips](0-2.jpg)

3. Access the administration console of SAP Cloud Identity Services tenant using one of the URL formats below:
  * https://your-ias-tenant.accounts.ondemand.com/admin
  * https://your-ias-tenant.accounts.cloud.sap/admin              
  **Note**: Substitute your-ias-tenant with your actual tenant's name.
4. Authenticate using an administrator user.</br>                
![configure_ips](0-1.jpg)

5. From the menu, access **Identity Provisioning >> Source Systems**.</br>
![configure_ips](2.jpg)

6. Under **Source Systems** click **+Add** icon.</br>
![configure_ips](3.jpg)

7. Click **Browse** to import a pre-defined source system configuration.</br>
![configure_ips](4.jpg)

8. Select the **SuccessFactors-SF-Company-ID-Joule.json** file downloaded earlier and click **Open**.

9. Update the **System Name** field and put in your SuccessFactors system company ID and click **Save**.</br>
![configure_ips](5.jpg)

10. Click on **Properties** and replace the placeholder values with appropriate values for your setup using the information below:
  * **sf.company.id** : Replace with placeholder value with your actual SuccessFactors CompanyID.  For e.g. DemoCompany in my screenshot.
  * **URL** : Specify the **mTLS Certificate Server** api URL corresponding to your SuccessFactors instance.  For e.g. https://api8.cert.successfactors.com in my setup.  Refer to the preparation steps if unsure of what URL to use.  DO NOT forget to include **.cert** in the URL.
  * **sf.user.filter** : Update the **userName** filter to include your own SuccessFactors users.  Use **or** to add additional users who should be replicated into  Work Zone when this job is run later.             
**Note**: The reason for using a filter for specific users is so that we don't replicate the full SuccessFactors user base into Work Zone.  This setup will make it easier to test with few users first before rolling out to larger number of users.  The userName part of the filter can be removed later if full user synch is required.</br>
  ![configure_ips](6.jpg)

11. Click **Save**.
12. Click on **Outbound  Certificates** icon and click **Download Certificate** icon.  This will download the certificate into your downloads folder.
**Note**: This certificate will need to be imported into SAP SuccessFactors system in later steps.</br>
![configure_ips](6-1.jpg)


## **Configure SAP Build Work Zone, standard edition as a target system in Identity Provisioning**

1. From the menu, access **Identity Provisioning >> Target Systems**.</br>      
![configure_ips](7.jpg)

2. Under **Target Systems** click **+Add** icon.</br>                 
![configure_ips](8.jpg)

3. Click **Browse** to import a pre-defined target system configuration.
4. Select the **WorkZone_Target_ForJoule.json** file downloaded earlier and click **Open**.</br>      
![configure_ips](9.jpg)

5. From the **Source System** dropdown make sure to select the source system created earlier.  For e.g. **SuccessFactors - DemoCompany-Joule** in my screenshot.</br>  
![configure_ips](10.jpg)

6. Switch to the **Properties** tab and update the following placeholders with the appropriate values for your system:
 * **cflp.providerId**: <-- **ID** of the content provider create in Work Zone.  **SFSF** in my case -->
 * **URL**: <--**portal-service** url from the key file downloaded earlier when we create Work Zone instance in BTP-->
 * **OAuth2TokenServiceURL**: <--**url** field from the key file downloaded earlier.  Make sure to add **/oauth/token** to end of the URL-->
 * **User**: <--**clientid** from key file downloaded earlier-->
 * **Password**: <--**clientsecret** from key file downloaded earlier--></br>
 ![configure_ips](11.jpg)
 
 7. Click **Save**.
 
## **Import the certificate into SAP SuccessFactors**

1. Log into SAP SuccessFactors using an admin user.
2. Access **Security Center** from search area.</br>
![configure_ips](12.jpg)

3. Click **X.509 Certificate Mapping** tile.                       
**Note**: If the tile is not visible, make sure you have the appropriate permissions assigned to your SuccessFactors admin user.  See preparation section.</br>         
![configure_ips](13.jpg)

6. Click the **Add** button.</br>                         
![configure_ips](14.jpg)

7. Specify the following information and **Save** your configuration:
 * Configuration Name: <--Name of your choice--->
 * Integration Name: Select **Identity Provisioning Service** from the dropdown.
 * Certificate File: Upload the certificate file downloaded from the SuccessFactors source system we created in earlier steps.
 * Login Name: <--Leave blank--></br>
![configure_ips](15.jpg)

8. Confirm the certificate is successfully uploaded in SuccessFactors.</br>              
![configure_ips](16.jpg)
