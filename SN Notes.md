# ServiceNow Notes

[TOC]

## Labels

- To fix label with wide spaces, add some instructions to the label to fill the widespace

## Checkboxes

- Label HTML not working if checkbox is inserted right after the label.

  - **Solution**: Put the label in a container

- Checkbox cannot be made mandatory via client scripts or UI policies

  - **Solution**: make it manatory in the variable definition

- If checkbox in containers, "Option" label will appear on top of each section
  - **Solution**: Custom empty label hide it, UI policy will hide checkbox as well though

## Creating new Tables

1. Tables & Columns
2. Label - no spaces required
3. Name - Auto Filled
4. Create module - if you want it to be displayed on the menu or not
5. Create access controls - false if you don't need ACL to restrict this table
6. Display value - reference to this table will show this field

## Notifications

- When testing notifications, be careful of the random users you select, some may not have an email address filled in

- Sent to event Creator - Unselecting this option will remove the event creator from the recipient list at the END of the notification generation

## Email Inbound Actions

- [Get the latest email reply](https://glassputan.wordpress.com/2012/03/08/managing-email-replies/)
- [Filter out logos from email reply](https://community.servicenow.com/community?id=community_question&sys_id=414a47a9db5cdbc01dcaf3231f9619f3)

## Before Business Rules

- Don't use gr.update() before insert, or you may get a duplicate database error
- Be careful using GlideRecord Queries on before insert business rules. Records may not have been created in the database yet. Instead try to dot-walk to related records

## ACL

- need both ROW ACL AND Variable to make a field show
  - Ex. change_task ACL + change_task.u_validation need to be enabled to see u_validation

## Workflows

- Inserting a record that matches the state of a wait condition in a worfklow will not advance the workflow. Need to update the workflow via script.

```javascript
// Manually update the workflow to continue
new Workflow().runFlows(rec, "update"); // rec is the GlideRecord
```

- Manually restart a workflow, still need to go to workflow context and nudge

```javascript
var ritm = new GlideRecord("sc_request");
ritm.get("254b838ddbfbeb001691f5361d9619ec");
new Workflow().restartWorkflow(ritm);
```

## CrossScope function calls

- If to call a script in another scope use `Scope.ScriptIncludeName()`
  - example: `global.inboundActionUtil()`
- Within the script include, use `this.functionName()` to call local function if the original call is from another scope
