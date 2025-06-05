
# Part A - Manage Copilot features
___
## Section 0 - Introduction

Learning outcomes:
1. Plan and configure Copilot in Customer Service for agents
2. Enable Ask a Question in Copilot in Customer Service, including filtering content
3. Configure case and conversation summaries in Copilot in Customer Service

What can Copilot be used for?
- Responding to questions
- Composing emails
- Drafting chat responses
- Summarising conversations (only in the Customer Service workspace app) and cases

## Section 1 - Turn on Copilot in Customer Service 

### i - Turn on Copilot in Customer Service and make it available to agents
- An administrator must go into the **Customer Service admin centre** app to turn on and configure Copilot in order for sales representatives to use it.
- There are two primary sections for setting up Copilot:
	- Copilot help pane
	- Summaries
- To turn on Copilot for the Customer Service workspace, you need to select **Make Copilot available to agents**.
- Any user added to one of the default agent profiles will then have access to Copilot features.
- To limit access to Copilot, you can create a custom agent experience profile and enable only the required features, then assign the custom profile to agents. To navigate to the agent experience profiles use one of the following:
	-  **Agent experience** > **Workspaces**
	-  *Copilot for questions and emails > **Agent access** > **agent experience profiles**
- Regarding limiting Copilot access, once you have reached the agent experience profiles page, on the Productivity Pane, turn on the **Copilot help pane** toggle so that agents have access to that feature (suggest a response, ask a question, write an email)
- You can also select features such as 'Ask a question, Scan customer conversation' etc. in Copilot AI features.
- Out of the box, only the Customer Service Representative role can use Copilot features. You need to give users with custom roles certain privileges detailed here: https://learn.microsoft.com/en-us/dynamics365/customer-service/administer/configure-copilot-features#make-copilot-available-to-agents
- After turning on **Make Copilot available to agents**, you'll get two extra options:
	- For customer chat - Copilot has answer questions for the user when working in the customer chat window
	- For email - Copilot can help with writing your emails

### ii - Configure knowledge sources
- Copilot uses the content attribute of the knowledge article table to generate responses. This can't be customised.
- You can turn on knowledge base (stored in the Knowledge Management module of MS Dynamics) to allow Copilot to use internal knowledge base resources to generate responses to **ask a question** and **draft an email** in the Copilot help pane and the rich text editor
- You can add up to 5 trusted webpages as sources, which Copilot can read two levels down from (https://www.example.com/page1/subpage1)

### iii - Case and conversation summaries

- Copilot can provide representatives with summaries of case records and conversations
- Administrators can configure the Copilot summarisation features in the Customer Service admin centre app by going to **Agent Experience > Productivity > Summaries**.
- You can configure when summaries are generated:
	- When an agent joins a conversation
	- When a conversation ends (there is also a sub-option allowing agents to create a case with a button in the summary)
	- On demand with a button
- You can turn on **Agent experience data** to gain an understanding about how agents are using Copilot

## Section 2 - Manage fields for case summaries

- The case summary feature provides a digestible view of a case record which is generated using source case fields. 
- You can modify these fields to improve the context and accuracy of the results.
- For example you might want to include the **Case Origin** field in the summary so that the representatives understand the context of the channel that the initial communication was made on.
- You can also select custom fields that you want Copilot to use for generating responses.
- Modify which fields Copilot uses for case summaries in the **Customer Service admin centre** app by going to **Agent Experience > Productivity > Summaries** and in the **Case summaries** section, select **Manage Data**.
- Step by step guide to modifying the fields used to generate case summaries:
	1. Use one of the following navigation options:
	    - **Agent Experience** > **Productivity** > **Summaries**
	    - **Operations** > **Insights** > **Summaries**
	2. In **Summaries**, for **Case summaries**, select **Manage Data Attributes**. The **Data Attributes** pane shows the attributes that Copilot uses to generate a summary.
	3. Complete the following steps.
	    1. Select the attributes that Copilot uses to generate case summaries. Copilot considers the selected fields only when it's generating a summary.
	    2. To change the default attributes, select a different source table and column. For the **Customer** attribute, for example, you might select **Account** as the source table and **Account Name** as the column. When Copilot generates a summary, it uses the value in the **Account Name** column of the **Account** table as the customer contact instead of **Case** and **Contact**.
	    3. Select **Save and Close**.
	4. Select **Save**.
- The following fields are used by Copilot to generate case summaries:
	- Customer
	- Case title
	- Case type
	- Subject
	- Case Description
	- Product
	- Priority
	- Case notes
	- Email content
	- Conversation Summary
- You can customise how and what data Copilot uses in the **Manage data for case summaries** screen
- Each item includes the following settings:
	- **Include** - Specifies whether Copilot should include the item in the case summary.
	- **Description** - Identifies the type of information that the item includes.
	- **Record type** - Specifies which record type (table) in Microsoft Dynamics 365 to use.
	- **Data field** - Specifies which field for the selected record type contains the data that you want to use in the summary.
- You can change the mapping so that for example Case title maps to Case Description instead.
- Some things to consider:
	- The Case notes field can be any entity related to the Case table.
	- The CSR or whatever tole your user is must have Read permissions configured for related entities in order for Copilot to be able to generate a summary.
	- **Email content** and **Conversation Summary** can't be modified.
	- You can't add more attributes

## Section 3 - Customise Copilot conversation summaries

- You can customise conversation summaries so that representatives don't need to edit them.
- Go to the **Customer Service admin centre** app and then **Agent Experience > Productivity > Summaries**. Under the **Live conversation summaries** section, select the **Manage format** link.
- You can choose either the **Paragraph** or **Structured** summary format. The default selection is Paragraph, which allows for no customisations or tailoring. If you want to customise the summary format, select **Structured**.
- In **Structured** format, you can specify which information should be included in the summary such as:
	- **Root cause** - The reason why the issue occurred.
	- **Customer issue** - The problem or question that the customer has.
	- **Troubleshooting steps** - Steps that the representative took to resolve the issue.
	- **Outcome** - Notes and tasks that are related to the steps that the representative tried.
	- **Error codes** - Technical codes that display to the customer for errors
- You can also arrange the order of the information.
- To omit any mention of a piece of information that isn't present in the conversation, select the **Remove information from the summary that can't be found** toggle.

## Section 4 - Display the Copilot case summary on custom case forms

- If you want to use Copilot case summary on custom forms, you need to add them manually.
- You can also turn on Copilot features for custom apps in your organisation by going to **Power Apps** > **Solutions** > **Select the solution you want** > **Add existing > More > Setting**, then in the **Add existing setting definition** dialog, select one or more settings that you want to add. Then select next, and in the **Selected setting definition** dialog, for each setting you have selected, you will have the option to **Include setting definition**. You can also **Include setting environment value** for each setting, if one exists. Select **Add** to add the setting definition and/or setting environment values.
- Then you need to go to the **Edit Customer Service Copilot Enabled** pane, in the  **Setting app values** section, set the **New app value** to **Yes** for a required app.
- Follow these steps for updating a setting definition (the above step):
	1. Sign in to [Power Apps](https://make.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc).
	2. In the navigation pane, select **Solutions**. If the item isn’t in the side panel pane, select […More](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/intro-maker-portal#1--left-navigation-pane) and then select the item you want.
	3. From the list of solutions, open the solution you created the setting in.
	4. In the tree view, select **Settings > Setting definitions**.
	5. Select the setting definition that you want to update.
	6. In the **Edit setting** dialog, update values for any of the properties you want to change.  
	    Note that some properties can't be updated after a setting has been created. Additionally, in most cases you will not be able to update settings definitions that you don't own.
	7. When you are done updating the values for the properties, select **Save**.
# Part B - Create and manage SLAs for cases
___
## Section 0 - Introduction

Learning outcomes:
1. Configure SLA settings
2. Implement actions by using Power Automate
3. Apply SLAs
4. Create and manage SLA items including key performance indicators (KPIs), warning actions, success actions, and applicability
5. Configure a timer control on a form
6. Create SLA KPIs

Customer service-based organisations track customer satisfaction through key performance indicators (KPIs). A KPI is a measurable value that demonstrates how effectively a company is achieving key business objectives. Organisations use KPIs to gauge success and improve customer service.

Some examples of KPIs include:
- **Customer Satisfaction Score (CSAT)** - This KPI is the most popular, and you can capture it by asking customers about their satisfaction with your business, product, or service.
- **Net Promoter Score (NPS)** - Measures the likelihood that your customers will refer you to someone else.
- **First Response Time** - Measures quickness of initial customer response when an issue is created.
- **Call Resolution Time** - Measures how quickly you're resolving customer issues.
- **Customer Retention Rate** - Ability to keep paying customers over a set period of time.
- **Employee Engagement** - Measures the level of employee commitment and connection to your organisation.

- Dynamics 365 Customer Service includes the ability to define service-level agreements (SLAs) to help organisations meet the desired service levels when providing support to customers.
- You can use SLAs to track common KPIs such as **First Response Time** and **Call Resolution Time** for each case that is submitted.
- You can also create custom KPIs to keep track of business-specific items.
- Each SLA is configurable to model different KPIs based on different case attributes. A single SLA will include multiple SLA detail lines that describe the KPI that you're tracking and the actions that are associated with it.
- Each detail line defines the following parameters:
	- **SLA KPI** - Specifies the KPI that you're measuring.
	    **Example**: First Response By or Resolve By
	- **Applicable When** - Defines the conditions that need to be met for the item to apply to the case.
	    **Example**: If a case's **Service Level** is **Gold** and the **Case Priority** is set to **High**
	- **Success Criteria** - Defines what successful resolution of the KPI looks like.
	    **Example**: If the **First Response Sent** field is set to **Yes**
	- **Success Actions** - Defines an action that should be done if the KPI is met.
	    **Example**: Update the case record to indicate that a first response was sent
	- **SLA Item Failure** - Defines how long to wait until an item is considered to fail or not be met.
	    **Example**: No first response communication is done within one hour of case creation
	- **Failure Actions** - Defines action(s) that should be done if the KPI isn't met.
	    **Example**: Escalate case to the **Escalation Queue** and notify service manager
	- **SLA Item Warning** - Defines how long to wait to provide a warning that a work item is in jeopardy of not being met.
	    **Example**: No first response communication is done within 30 minutes of case creation
	- **Warning Actions** - Defines warning action(s) that should be done if the KPI is in jeopardy of not being met.
	    **Example**: Send reminder email to responsible agent

An example of a typical SLA for different types of customers based on their promised level of service:

![[Pasted image 20250108235216.png]]

- This particular SLA is set up to automatically send emails and update case records based on the type of customer and the priority of the case, as well as the timing of a customer service representative's actions. This SLA will
	- Send a warning email to the owner of the case if they’re in danger of not meeting the **First Response by** KPI.
	    - Send an email to the customer service representative and the customer service manager if they fail to meet the **First Response by** KPI.
	    - Update the case record by setting the **SLA First Response Sent** field to **No**.
    - Send a warning email to the owner of the case if they’re in danger of not meeting the **Resolve by** KPI.
	    - Send an email to the customer service representative and the customer service manager if they fail to meet the **Resolve by** KPI.
	    - Update the case record by setting the **SLA Resolution** field to **No**.
- SLAs can also consider other factors such as business hours, business closures. For example if a KPI requires a 4-hour response but the business closes in 1 hour, then the response time required by the SLA before a warning email is sent will be 3 hours after the start of business hours on the next day.
- You should also consider time spent waiting for the customer (e.g. the customer is required to give a password but spends 3 hours finding it - this might not have to count against the 4-hour KPI).

## Section 1 - Work with business closures and working hours

- When a customer opens a case, you need to identify the hours to associate the case with and apply the correct SLA.
- Make sure to identify the different working schedules of employees in your organisation (for example factor in time zones and business holidays).

### i - Holiday calendars

- You can alert your SLAs to planned business closures by creating a holiday calendar and adding it to your service schedule.
- Always create a holiday calendar before a service calendar as the latter can be associated with a holiday calendar.
- In the Customer Service admin centre, go to **Calendar** > **Holiday calendar**. Make sure to assign it a name, such as UK Bank Holidays. Consider holidays in different regions that your business operates in.
- Once you've created a holiday calendar, you can add holidays to it. A holiday includes the following fields:
	-  **Name** - Name of the holiday
	- **Start Date** - Date when the holiday begins
	- **End Date** - Date when the holiday ends
	- **Duration** - Total duration of the holiday
- Each individual holiday needs to be defined (there are no recurring holidays), so New Year's Day needs to be added for each year separately (as NYD 2026, NYD 2027, NYD 2028 etc.)
- You can import an excel file and associate the holidays within to a holiday calendar

>[!note]
>Remember that holiday schedules are used so that SLAs are not affected when your organisation is closed. They are an SLA-specific feature of MS Dynamics.

Here is a step-by-step process of how to create a holiday schedule:

You can create a holiday schedule in the Customer Service admin centre app.
1. Make sure that you have the Customer Service Manager, System Administrator, or System Customiser security role or equivalent permissions.
    - Follow the steps in [View your user profile](https://learn.microsoft.com/en-us/dynamics365/customerengagement/on-premises/basics/view-your-user-profile). Don’t have the correct permissions? Contact your system administrator.
2. In the site map of Customer Service admin centre, select **Calendar** in **Operations**. The **Calendar** page appears.
3. In the **Holiday calendar** section, select **Manage**. The **All Holiday Schedules** view is displayed. You can switch between various system views using the dropdown list.
4. Select **New** and in the **Create Holiday Schedule** dialog box, enter a name and description for the holiday, and then select **Create**.
5. When the holiday opens, select **New** in the **Holidays** grid to add the holiday to your customer service calendar.
6. In the **Add Holiday** dialog box, specify the **Name**, **Start Date**, **End Date**, and **Duration** of the holiday, and then select **OK**.

The holiday is created and associated with your customer service calendar. After the customer service schedule is associated with an SLA, your SLA during business hours isn't affected. More information: [Define service level agreements](https://learn.microsoft.com/en-us/dynamics365/customer-service/administer/define-service-level-agreements)

You can also create a holiday schedule in the Customer Service App:

1. Make sure that you have the Customer Service Manager, System Administrator, or System Customiser security role or equivalent permissions.
    - Follow the steps in [View your user profile](https://learn.microsoft.com/en-us/dynamics365/customerengagement/on-premises/basics/view-your-user-profile). Don’t have the correct permissions? Contact your system administrator.
2. Go to **Settings** > **Service Management**
3. Select **Holiday Schedule**.
4. Select **New** and in the **Create Holiday Schedule** dialog box, enter a name and description for the holiday, and then select **Create**.
5. In the list of holidays, select the holiday you created.
6. When the holiday is open, select **New** to add the holiday to your customer service calendar.
7. In the **Add a Holiday** dialog box, specify the name and select the time of the holiday, and then select **OK**.

The holiday is created and associated with your customer service calendar. After the customer service schedule is associated to an SLA, then your SLA during business hours isn't affected. More information: [Configure service-level agreements](https://learn.microsoft.com/en-us/dynamics365/customer-service/administer/define-service-level-agreements)
### ii -  Customer Service calendar

After defining holiday schedules (always do this first) define a service calendar by going to **Calendar** and selecting **Customer Service calendar**.

When defining the work hours for a customer service calendar, you can define the following options:
- **Are the same for each day** - Select this checkbox to set work hours to be the same for each day. Selecting the working hours will allow you to define the work hours, including breaks. 
- If you don't select **Are the same for each day**, you'll be able to select hours for each individual day.
- **Holiday Schedule** - Define if this service calendar observes holiday calendars. If you select to observe holidays, you can select which holiday schedule to use.
- **Time Zone** - Define the time zone for this service calendar.

Here is a step-by-step process for defining work hours by creating a customer service schedule:

1. Make sure that you assign the Customer Service Manager, System Administrator, System Customiser security role, or equivalent permissions at the user level only.
    #### Check your security role
    - Follow the steps in [View your user profile](https://learn.microsoft.com/en-us/dynamics365/customerengagement/on-premises/basics/view-your-user-profile).
    - Don’t have the correct permissions? Contact your system administrator.
2. Navigate to the Customer Service admin centre app, and perform the following steps:
3. In the site map, select **Calendar** in **Operations**. The **Calendar** page appears.
4. In the **Customer service calendar** section, select **Manage**. The **All Customer Service Calendars** view is displayed. You can switch between various system views using the drop-down list.
5. To create a customer service schedule, select **New**.
    To edit an existing schedule, select the schedule in the list of records, and then select **Edit** on the command bar.
6. In the **Name** field of the **Create Customer Service Schedule** dialog, enter a meaningful name for the schedule such as “APAC Customer Schedule", and then select **Create**.
7. In the **Weekly Schedule** dialog, follow these steps:
    1. For work hours, select one of these options:
        - **Are the same each day**: The schedule is the same for every day of the week. After you select this option, to select the days of the week that the customer support is available, select **Set Work Hours**.
            
            To set the work hours for the days, select **Set Work Hours**. For more information, see [Define work hours for the customer service schedule](https://learn.microsoft.com/en-us/dynamics365/customer-service/administer/create-customer-service-schedule-define-work-hours#define-the-work-hours-for-the-schedule).
            
        - **Vary by day**: The new schedule is different for one or more days of the week. After you select this option, select the days of the week that the customer support is available, and also specify the work hours for each day.
        - **24 x 7 support**: The customer support is available 24 hours a day and all days of the week.
    2. For **Work Days**, select the checkbox for each day that the customer support resources will be available and working.
    3. For **Holiday Schedule**, select **Observe** to specify when your service organization will be closed.
        
        If you selected **Observe**, select a holiday schedule from the lookup box. More information:[Set up a holiday schedule](https://learn.microsoft.com/en-us/dynamics365/customer-service/administer/set-up-holiday-schedule)
8. In the **Time Zone** dropdown box, under **Select the time zone**, select the time zone in which your customer support resources will work. If applicable, the daylight saving time is taken into account for the selected time zone.
9. Select **Save**.

##### Define the work hours for the schedule
In the **Set Work Hours** dialog, complete the following fields, and then select **OK**.
- **Start**: Select the time the work day starts.
- **End**: Select the time the work day ends.
    To add a break in the work hours, like a lunch break, select **Add Break**, and then select the start and end time of the break.

## Section 2 - Create and define service-level agreements

### i - Create SLA KPIs
- In order to create a service-level agreement (SLA), you first need to create KPIs to be used in your SLA.
- Create SLA KPIs from the Customer Service admin centre by going to the **Operations** group, selecting **Service terms**, and then **Manage** in the **SLA KPIs** section.
- Defining an SLA KPI involves the following fields:
	- **Name** - Specifies the name of the SLA KPI that will be used to identify it in the application.
	- **Owner** - Defines the owner of the item. By default, it's set to the person who’s creating the item, but you can change it.
	- **Entity Name** - Specifies the Microsoft Dataverse table that the KPI will be associated with, such as the Case table.
	- **KPI Field** - Specifies the KPI field that will be used for this item. For example, if you're creating an SLA KPI to define the time within which a first response should be sent to the customer, select **FirstResponseByKPI** in the list. Out-of-the-box, the Case table includes two options that you can choose from: **FirstREsponseByKPI** and **ResolveByKPI**.
	- **Applicable From** - Defines the field that will be used to measure warning and failure times.
- **Applicable From** is an important field as it sets the time from which a KPI will start tracking. For example if you set it to **Created On**, an agent will have a certain amount of time to complete a task starting from the time that the case was created. However if you set the **Applicable From** field to **Modified On**, then the timer starts when the case record is updated.
- After defining an SLA KPI, you need to activate it so that it can be used in SLAs.

### ii - Create SLAs

- SLAs track and measure KPIs based on who the customer is and the specifics around the case.
- SLAs are created at an organisational level - not specific to an individual customer.
- SLAs can be applied to customer cases manually or by attaching cases to an ***entitlement*** that has a specific SLA associated with in.
- Usually you would define a default SLA for an organisation that will be automatically applied in instances where no SLA has been defined for a case.
- SLAs are containers for individual SLA items that specify which KPI is being used along with a success criteria.
- When creating an SLA, you initially need to provide:
	- **Name** - Specifies the name of the SLA.
	- **Primary Entity** - Defines the Dataverse table that the SLA will apply to.

## Section 3 - Define SLA items

- SLAs can contain multiple SLA items:
![[Pasted image 20250109005556.png]]

- **Gold Customer-First Response By** - If case **Service Level** = _**Gold**_, then the **First Response** should be within _**one hour**_.
- **Gold Customer-Resolve By** - If case **Service Level** = _**Gold**_, then the **Resolve case** should be within _**four hours**_.
- **Silver Customer-First Response By** - If case **Service Level** = _**Silver**_, then the **First Response** should be within _**two hours**_.
- **Silver Customer-Resolve By** - If case **Service Level** = _**Silver**_, then the **Resolve case** should be within _**one day**_.
- **Bronze Customer-First Response By** - If case **Service Level** = _**Bronze**_, then the **First Response** should be within _**four hours**_.
- **Bronze Customer-Resolve By** - If case **Service Level** = _**Bronze**_, then the **Resolve case** should be within _**two business days**_

- In the above example, each KPI that you want to track for each service level tier would be added to the SLA as its own SLA item.

- Each SLA item requires the following information:
	- **Name** - Specifies the name of the SLA item. _(This field is required.)_
	- **KPI** - Specifies the SLA KPI that you're measuring. For example, you can select the **First Response By** or **Resolve By** SLA KPIs that you previously defined.
	- **Allow Pause and Resume** - Specifies if the SLA timer can be paused and resumed. After the timer has resumed, the amount of time that it was paused for won't affect the SLA timer.
	- **Business Hours** - Specifies if a Customer Service calendar is available for you to apply to the SLA item to affect how the items are calculated.
	- **Applicable When** - Defines the conditions that need to exist on the record that the SLA is running against or a related record for the specific SLA item to be applied to the record (such as a case **Service Level** being set to **Gold**).
	    - Be aware of using a field that could change frequently when you’re defining applicable **when** conditions, which can affect system performance.
	    - Only active records are available for you to select as condition or action arguments.
	- **Success Conditions** - Defines what a successful resolution of the defined KPI looks like (such as a specific field on the current or related record that’s being updated).

### SLA warning and failures
- Define when to warn and when to fail the SLA (e.g. If the SLA item requires a response in 1 hours, it could warn after 30 minutes and should fail after 1 hour).

### Actions
- You can use actions to perform tasks automatically in response to the success criteria of an SLA item being met or failed.
- First save a detail item, then define actions by selecting the **Configure Actions** button which will open Power Automate as that is how actions are implemented.
- The Power Automate flow will initially include two steps. DO NOT DELETE OR UPDATE THESE STEPS.
- The second of these steps is a switch. You can edit the individual switch steps, representing the actions you can complete
- You can define three types of actions:
	-  **Is Near Non Compliance** - Defines what action(s) should be implemented if the success criteria are in jeopardy of not being met within the specified warning time.
	     Important!
	    Success actions are only available for enhanced SLAs.
	- **Is Succeeded** - Defines what action(s) should be performed if the success criteria are met.
	- **Is Non-compliant** - Defines what action(s) should be completed if the success criteria aren’t met within the specified failure time.

### Work with multiple SLA items
- Each SLA is evaluated in the order that's listen in the SLA items sub-grid.
- If multiple SLA items reference the same related field on a specific record, they're also evaluated in the specified order.
- The first SLA item that's applicable for each related field will the the one that's applied.
- You can modify the order by using the arrows in the sub-grid. Put the more specific ones higher on the list.
![[Pasted image 20250109010956.png]]

## Section 4 - Define Custom KPIs

- The case table is automatically set up for use with SLAs.
- In order to use SLAs with any other table, you can go to the Power Apps maker portal, expand **Data/Dataverse**, and then select the table you want to enable for SLAs. Then go to **Settings > Advanced Settings > Make this table an option when**. Select the **Setting up service level agreements** checkbox to enable it. After you’ve enabled the setting, save your changes to the table.
- If the above doesn't work, go to the specific table (e.g. Email), in the Table Properties card select **Properties** click advanced then scroll down to **Make this table an option when** and selecting Setting up **service level agreements**.

### Create custom KPIs

- Define custom KPIs by creating a lookup column (1:N relationship) with the SLA KPI Instance table in Power Apps by going to the table you want to define the KPI for and then selecting Columns.
- Then, you can create a new lookup column for that table and set the lookup field to use the SLA KPI Instance table.
- It will then be available for you to select it as a KPI field when you're creating a new SLA KPI that's associated with that table.
## Section 5 - Manage SLAs

- Out of the box, the case record doesn't have a lookup field to the SLA entity, so an SLA isn't applied to a case unless it's set at the default SLA for your organisation or associated with entitlement.

| Action                                          | Applicable SLA                                                                                                                                                                                                                                                                                                                              |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Case created with no entitlement associated** | The organization's default SLA will be applied to the case. When no default SLA exists, no SLA will be applied to the case.                                                                                                                                                                                                                 |
| **Entitlement is associated to the case**       | When the entitlement has an active associated SLA, the associated SLA will be applied to the case. When the entitlement doesn't have an active associated SLA, the default SLA will be applied to the case. When the entitlement doesn't have an active associated SLA, and no default SLA exists, then no SLA will be applied to the case. |
### Pause and resume SLAs
- If you want to pause an SLA, for example when waiting on a customer for details, you can do so in the Customer Service admin centre by defining **Pause** and **Resume** statuses for a specific table such as Cases
- Go to **Service Terms** and select **Manage** next to **Other settings**.
- This will allow you to pause and resume SLAs based on the entity status (e.g. **On Hold** or **Waiting for Details**)

### Timer control
- Add a timer control to an entity through the form customisation screen to provide countdown information for any defined KPI on that entity.
- When a timer control is added, you can set up the following parameters:
	- **Failure Time Field** - Defines the date/time-based field that will be used for calculating success or failure, such as the **Created On** field. (Required)
	- **Success Condition** - Defines the field and field value that determine successful resolution, such as the **First Response Sent** field being set to **Yes**. (Required)
	- **Failure Condition** - Defines the field and field value that determine a failure for the time, such as the **First Response Sent** field being set to **No**.
	- **Warning Condition** - Defines the field and field value that determine when a warning should be triggered.
	- **Cancel Condition** - Defines the field and field value that determine when the timer should be canceled, such as the **Status Reason** being set to **Resolved**.
	- **Pause Condition** - Defines the field and field value that determine when the timer should be paused, such as the **Status Reason** being set to **Waiting for Details**.

You can add and define timer controls whenever you need to use countdown capabilities. The timer will change color to reflect the different statuses, such as Green for all is good, Yellow for warnings, and Red for failures.

## Section 6 - Add a timer control for SLA-enabled entities

The SLA Timer control displays countdown timers for SLA KPIs, helping customer service representatives track time and stay within SLA limits. To implement this, follow these key steps:

1. **Add SLA Timer Control to Entity:**
    
    - After configuring SLAs and KPIs, add the SLA Timer to the relevant entity form (e.g., Case).
    - Use the Power Platform environment, navigate to Advanced Settings > Customizations, and select the entity and form.
    - Insert a section and a subgrid, and set the data source to display only the relevant SLA KPI Instances.
2. **Configure Timer Control:**
    
    - Add the SLA Timer control in the Controls tab.
    - Set the update frequency (default is 30 minutes) for optimal performance.
    - Save and publish the solution.
3. **Handle Expired SLAs:**
    
    - Enable negative countdown to show elapsed time after SLA expiration by setting the "Turn on negative countdown" option.
4. **Customize Labels:**
    
    - Edit labels for various SLA statuses (e.g., Noncompliant, Paused) and support multiple language codes.
5. **Out-of-the-Box Timer on Case Form:**
    
    - The timer shows status (Succeeded, Expired, Canceled) and updates color as time progresses. It starts counting up after expiration.
    - For non-RESTful actions, ensure all relevant conditions (Success, Failure, Warning) are properly configured.
6. **For Enhanced SLAs:**
    
    - These timers differ from standard SLAs; see the article on tracking time against enhanced SLAs for more details.
7. **Additional Notes:**
    
    - Timers can be added to any main form of entities, but not to Dynamics 365 for tablets.
    - You can have multiple timers for different KPIs.

## Section 7 - Concise guide to SLAs

Here’s a simplified, step-by-step guide to creating SLAs (Service Level Agreements) in Microsoft Dynamics 365 Customer Service:

---

### **1. Prerequisites**
Ensure you have:
- The **System Administrator, System Customizer, or Customer Service Manager** role.
- Required permissions for custom entities like SLA KPI and Connector.
- A **Power Automate license** for creating automated actions.

---

### **2. Create SLA KPIs**
SLA KPIs (Key Performance Indicators) help track performance, like "First Response Time" or "Resolution Time."

1. Go to **Customer Service Admin Center** → **Service Terms** in Operations.
2. In the **SLA KPIs** section, click **Manage** → **New SLA KPI**.
3. Fill in details:
   - **Name**: Name your KPI.
   - **Entity Name**: Choose the entity to track (e.g., Case).
   - **KPI Field**: Select the relevant KPI (e.g., `FirstResponseByKPI`).
   - **Applicable From**: Set the start time (e.g., `Created On`).
4. To configure pause criteria:
   - Toggle **Override Criteria** to **Yes**.
   - Add conditions for pausing the SLA KPI.
5. Click **Activate** to finalize the KPI.

---

### **3. Create an SLA**
SLAs define the conditions and actions applied to entities like cases.

1. Go to **Customer Service Admin Center** → **Service Terms** → **Manage** under **SLAs**.
2. Click **New SLA**.
3. Fill in details:
   - **Name**: Name your SLA.
   - **Primary Entity**: Select the entity (e.g., Case).
   - **Description**: Provide a brief description.
4. Save the SLA to enable the SLA Items section.

---

### **4. Add SLA Items**
SLA Items specify individual conditions and actions.

1. Open the SLA you created and click **New SLA Item**.
2. Fill in details:
   - **Name**: Name the SLA item.
   - **KPI**: Select the related KPI.
   - **Allow Pause and Resume**: Enable if needed.
   - **Business Hours**: (Optional) Set work hours.
3. Define conditions:
   - **Applicable When**: Specify when the SLA applies.
   - **Success Conditions**: Define what makes the SLA successful.
   - **Pause Configurations**: Add pause conditions (if enabled).
   - **Warn and Fail Durations**: Set warning and failure times.
4. Save the SLA item.

---

### **5. Configure Actions in Power Automate**
Automate notifications and actions for SLA milestones.

1. On the SLA item page, select **Configure Actions**.
2. In Power Automate, configure:
   - **Is Nearing Non-Compliance**: Set actions like sending a warning email.
   - **Is Succeeded**: Define actions when the SLA succeeds.
   - **Is Non-Compliant**: Define actions when the SLA fails.
3. Save and exit Power Automate.

---

### **6. Activate the SLA**
1. Return to the SLA page.
2. Activate the SLA to make it available for use.

---

### **7. Apply SLAs**
To apply SLAs to cases or other entities:
- Link the SLA to the relevant entity (e.g., Case).
- Ensure users have permissions to track SLA performance.

---

This guide breaks down the complex process into actionable steps. Let me know if you need further clarification on any part!
