Before you can activate Joule for SuccessFactors, there are certain number of pre-requisites that must be met.  This section describes those pre-requisites and outlines some details that need to be captured prior to running through the activation steps.

## 1. Validate Global User ID of SuccessFactors User

In order to use Joule, SuccessFactors users must have a Global User ID(GUID) field populated in their SuccessFactors user profile and this GUID should match what's in the SAP Cloud Identity Authentication (IAS) user profile.  This should already be in place if SuccessFactors integration to SAP Cloud Identity Services was done following best practices and a provisioning job was run to replicate user profiles from SuccessFactors to IAS. If the user replication job was not run in the past or if there were errors for certain users during the job execution, those users will not have the GUID field populated in SuccessFactors.  The **Manage Login Accounts** app can be used to visualize the GUID field in SuccessFactors and compare it to the user profile in IAS.  For more information on how to get access to the Manage Login Accounts application, follow [2859043 - Manage Login Accounts tool](https://userapps.support.sap.com/sap/support/knowledge/en/2859043).<br/>
![prepare_activation](5.jpg)

## 2. Create API User in SuccessFactors

An API user with **Allow Admin to Access OData API through Basic Authentication** permission is required in SuccessFactors.  This user will be used to create the BTP destinations in later steps.  If the user doesn't already exist, follow the steps in the **Create API User in SuccessFactors** card to create a new API user with the correct permissions.  If you plan to use an existing API user from SuccessFactors, confirm that there aren't any IP restrictions in place for the API user.  If such IP restrictions are in place then the IPs of BTP data center where Joule will be setup must also be added to the list of whitelisted IPs. To see the list of BTP datacenters and the corresponding IPs, follow [Regions and API Endpoints Available for the Cloud Foundry Environment](https://help.sap.com/docs/btp/sap-business-technology-platform/regions-and-api-endpoints-available-for-cloud-foundry-environment).  See [2253200 - How to restrict the API access of a specific user by IP addresses] (https://userapps.support.sap.com/sap/support/knowledge/en/2253200) for more information.<br/>

![prepare_activation](3.jpg)

## 3. Activate Employee Central Quick Links

Joule supports various use cases that are specific to Employee Central module in SuccessFactors.  To enable Employee Central specific use cases, Employee Central Quick Links should already be enabled in SAP SuccessFactors.  If Employee Central Quick Links are not already enabled, follow the steps in **Activate Employee Central Quick Links** card.
  
## 4. Permissions Required in SuccessFactors

* Permissions to schedule jobs in SuccessFactors.  See [Scheduled Job Manager](https://userapps.support.sap.com/sap/support/knowledge/en/2906009)
* Permissions to [**Security Center >> X.509 Certificate Mapping**](https://userapps.support.sap.com/sap/support/knowledge/en/3300596).
* Manage Permission Roles and Manage Permission Groups access.  See [Granting Administrators Access to RBP](https://help.sap.com/docs/SAP_SUCCESSFACTORS_PLATFORM/cdd844b5f0744d238284e937deb73f39/0aa4fbb7d9914a448d70238b321ab101.html).
* Permissions to access **Extension Center** (see [Enabling Extension Center](https://help.sap.com/docs/SAP_SUCCESSFACTORS_PLATFORM/6c9f794920b947648737d914a669f195/f1eface4d74545c1a29a9de970b686d9.html?version=2405))  in SAP SuccessFactors Admin Center that include:
  * **Admin Access to MDF OData API** permission from the **Metadata Framework** category.
    ![prepare_activation](7.jpg)
  * **Create Integration with SAP BTP** permission from the **Manage Extensions** on SAP BTP category.
    ![prepare_activation](6.jpg)

**NOTE:** If Extension Center is not visible in Admin Center, choose either one of 2 options documented in note [3414682 - Unable to View Extension Center even related permissions are granted](https://userapps.support.sap.com/sap/support/knowledge/en/3414682) to activate Extension Center.

## 5. Determine SuccessFactors Data Center

To validate whether SuccessFactors instance can be setup for Joule, you will need to find the correct data center for your SAP SuccessFactors instance.  To learn more, visit [2089448 - SuccessFactors Data Center Name, Location, Production Login URL, Production Domain Name, External Mail Server Details and Outbound IP addresses](https://me.sap.com/notes/0002089448)

For example, DC68 data center tenants the screenshot shows the corresponding SuccessFactors tenant URLs.<br/>
![prepare_activation](1.jpg)

**Note**: If your SuccessFactors tenant is using common super domain, make note of the **Pre CSD Migration URL** of your tenant as well.  This URL will be used when running the booster in the BTP Global Account.

## 6. Find API URLs for SuccessFactors tenant

We need to know the **API Server** and **mTLS Certificate Server** URLs for our SuccessFactors tenant.  This information can be found here: [List of SFSF API Servers](https://help.sap.com/docs/SAP_SUCCESSFACTORS_PLATFORM/d599f15995d348a1b45ba5603e2aba9b/af2b8d5437494b12be88fe374eba75b6.html)<br/>
![prepare_activation](2.jpg)

For example, for DC68 production SuccessFactors tenants the corresponding **API Server** and **mTLS Certificate Server** URLs for our SuccessFactors tenant are:   
* **API Server**: https://api4.successfactors.com/                         
* **mTLS Certificate Server**: https://api4.cert.successfactors.com

## 7. SAP Cloud Identity Services tenant(s) and SuccessFactors Details

To setup Joule, the SuccessFactors tenant should already be integrated to use SAP Cloud Identity Service.  As mentioned in the **Establish a Unified Joule Instance** mission that you completed to prepare for this mission, it's important that the BTP subaccount trust is setup with the same SAP Cloud Identity Authentication Service (IAS) tenant that is also used by SAP SuccessFactors.  To find out which IAS/IPS instance the SuccessFactors tenant is using access **Admin Center >> Monitoring Tool for Identity Authentication Service/Identity Provisioning Service Upgrade**.  Make a note of the **Identity Authentication Service Tenant URL** and **Identity Provisioning Service Tenant URL**.  Confirm that the BTP subaccount trust is setup with the same IAS tenant.  In addition, the date of when the activatation was done will be important to confirm the domain to use for BTP Subaccount trust to SAP Cloud Identity Authentication Service (IAS).<br/>
![prepare_activation](4.jpg)

## 8. Domain used for SuccessFactors trust with SAP Cloud Identity Services

In addition to knowing the URL of SAP Cloud Identity Services tenant, we also need to know which domain was used during the integration between SAP SuccessFactors and SAP Cloud Identity Authentication Service.  Use the table below to determine which domain is relevant for your integration:

| Criteria     | Domain to Use |
| ----------- | ----------- |
| SuccessFactors tenant already migrated to common super domain   | ias.accounts.**cloud.sap**     | 
| **Initiate the SAP Cloud Identity Services Identity Authentication Service Integration** job triggered **after Nov 20, 2023** in Upgrade Center   | ias.accounts.**cloud.sap**     | 
| **Initiate the SAP Cloud Identity Services Identity Authentication Service Integration** job triggered **before Nov 20, 2023** in Upgrade Center   |ias.accounts.**ondemand.com**     | 

Confirm the domain used to create the BTP subaccount trust in **Establish a Unified Joule Instance** mission aligns with either cloud.sap or ondemand.com domain based on the table above.

## 9. Validate SuccessFactors IP Restriction Management

Some of our SuccessFactors customers have IP restrictions in place to prevent user access to SuccessFactors from outside of that IP range.  For more information on IP Restriction Management in SuccessFactors follow this SAP Note: [2089414 - System: How to restrict access to SuccessFactors by IP address - IP Restriction Management](https://userapps.support.sap.com/sap/support/knowledge/en/2089414).  If such IP restrictions are in place then the IPs of BTP data center where Joule will be setup must also be added to the list of whitelisted IPs. To see the list of BTP datacenters and the corresponding IPs, follow [Regions and API Endpoints Available for the Cloud Foundry Environment](https://help.sap.com/docs/btp/sap-business-technology-platform/regions-and-api-endpoints-available-for-cloud-foundry-environment)<br/>

**WARNING**: DO NOT add the BTP IPs unless you are already restricting user access to SuccessFactors based in IPs.  Please read note 2089414 in detail before making any changes in the **IP Restriction Management** app in SuccessFactors.
