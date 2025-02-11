1. Login to SuccessFactors Application.
2. Navigate to **Admin Center**-> **Scheduled Job Manager**.
3. Under **Job Monitor**, set the **Job Type** filter to **Refresh Synthetic Group Data** and click **Go**.</br>
![Run_Synth_Group_Job](1.png)
4. Confirm the **SyntheticGroupRefreshJobType_<companyID>_deepLinkActivationPermission** has executed successfully at least once in your system.
![Run_Synth_Group_Job](1.png)
6. Enter the following information:
  * **Job Name**
  * **Job Type:** Refresh Synthetic Group Data
  * **Job Parameters:** Common Data Model Content Deep-Link Access User Group
  * **Occurrence:** Recurring
  * **Recurrence Pattern**: Daily
  * **Start Date**: Use current date and set to execute in few minutes
  * **End Date**: Time frame of your choice
  ![Run_Synth_Group_Job](2JobDetail.jpg)

5. Click **Submit**.</br>


**Note**: This job must be successfully executed once before proceeding to the next step.  Set the start time of job to execute so that it executes in the next few minutes.</br>
**Note**: This job execution would make sure all the users who has been assigned deep link specific access are refreshed and synced correctly during Identity Provisioning sync to SAP Build Work Zone application.
