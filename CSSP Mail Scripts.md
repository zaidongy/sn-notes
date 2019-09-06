# Notifications

## Notification Table Structure

The structure of the table is made from tables, rows, and cells, bascially wrap everything in a cell to format the notification. A cell is defined by the `<td></td>` tag, and cells can have other tables embedded within them. Here is the requisition form approval template as an example.

```html
<div style="background-color: #ffffff; padding: 0; margin: 0;">
  <table
    style="background-color: white; font-family: Arial;"
    border="0"
    width="640"
    cellspacing="0"
    cellpadding="0"
    align="center"
  >
    <tbody>
      <!-- HEADER -->
      <tr>
        <td>
          ${mail_script:cssp_common_header_hr}
          <table style="font-size: 12pt;" width="640">
            <tbody>
              <tr>
                <td>Hello ${approver.first_name},</td>
              </tr>
              <tr>
                <td>
                  Request ${sysapproval} submitted
                  by&nbsp;${sysapproval.opened_for} requires your approval.
                  Please approve or reject the request after reviewing the
                  details below.
                </td>
              </tr>
            </tbody>
          </table>
          <br />
          ${mail_script:cssp_notification_action_section_header}
          <table style="font-size: 12pt;" width="640">
            <tbody>
              <tr>
                <td>
                  <div>
                    To approve, click one of the buttons below. When the new
                    message appears, add any comments regarding your decision in
                    the body of the email, and press &ldquo;send.&rdquo; Please
                    do not edit the subject line or remove the reference in the
                    email body.
                  </div>
                  <div>&nbsp;</div>
                  <div>${mail_script:cssp_hr_case_approval_history}</div>
                  <div>&nbsp;</div>
                  <div>
                    ${mail_script:u_approval_approve button}
                    &nbsp;&nbsp;${mail_script:sysapproval_approver_script_1}&nbsp;
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
          <br />
          ${mail_script:cssp_notification_information_section_header}
          <table style="font-size: 12pt;" width="640">
            <tbody>
              <tr>
                <td>
                  <div>&nbsp;</div>
                  <div>${mail_script:cssp_hr_case_requisition_details}</div>
                </td>
              </tr>
            </tbody>
          </table>
          <hr style="margin: 5 0 5 0;" />
          <table style="font-size: 12pt;" width="640">
            <tbody>
              <tr>
                <td>
                  View request
                  ${mail_script:cssp_notification_record_link_from_approval}.
                </td>
              </tr>
              <tr>
                <td>${mail_script:cssp_notification_signature_hr}</td>
              </tr>
            </tbody>
          </table>
          <table
            style="background-color: #efefef; padding: 0 0 10 0;"
            width="640"
          >
            <tbody>
              <tr>
                <td
                  style="color: #777777; padding-right: 20px; font-size: 12pt;"
                >
                  ${mail_script:cssp_common_footer_hr}
                </td>
              </tr>
            </tbody>
          </table>
        </td>
      </tr>
    </tbody>
  </table>
</div>
```

## Add or remove sections from the table

To remove a section, we need to remove the entire table, and not just the row or the cell. For the example above, if we wanted to remove the information section of the notificaiton, we'd need to remove this whole block, including the `<br />` to exclude the line break. This will remove the header all the content related to that header.

If you want to add a section, do the reverse of the above and add this entire section to keep the structure consistent.

```html
<br />
${mail_script:cssp_notification_information_section_header}
<table style="font-size: 12pt;" width="640">
  <tbody>
    <tr>
      <td>
        <div>&nbsp;</div>
        <div>${mail_script:cssp_hr_case_requisition_details}</div>
      </td>
    </tr>
  </tbody>
</table>
```

## Styling

All styles must be done via **inline css** using the `style` property. Notifications cannot link to external stylesheets and `<style></style>` will be **removed** by ServiceNow before the email is generated.

Example:

```html
<td style="color: #777777; padding-right: 20px; font-size: 12pt;">
  ${mail_script:cssp_common_footer_hr}
</td>
```

## Mail Scripts

Mail scripts are used to generate content dynamically and enable reusability. They're extremely useful and should be used whenever there is block of content that will be used more than once. See below table for notes on each one I created (those that start with cssp). For the complex scripts, the code is documented so you can make customization on those as needed.

| Mail Script                                      | Comments                                                                                                                                                                              | Table That Can Use this script                                  |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| cssp_notification_escalation_response_link       | Generates the escalation link for HR Case Management                                                                                                                                  | sn_hr_core_case and extended                                    |
| cssp_add_notification_attachment                 | Adds the attachments of the current record to the notification. Formatted as just text links                                                                                          | Any                                                             |
| cssp_common_header_eis                           | Creates the EIS Header for the notification                                                                                                                                           | Any                                                             |
| cssp_common_header_hr                            | Creates the HR Header for the notification                                                                                                                                            | Any                                                             |
| cssp_current_attachment_links_with_header        | Adds the attachments of the current record with the HEADER to the notification. Header does not need to be created again in the template. Formatted in a table with Filename and Date | Any                                                             |
| cssp_notification_eis_record_link_from_survey    | Generates the record link the survey is for                                                                                                                                           | asmt_assessment_instance                                        |
| cssp_hr_case_onboard_new_details                 | Generates all the onboarding details for the manage non-employee form in a table format                                                                                               | sn_hr_core_case_operations                                      |
| cssp_common_footer_eis                           | Generates the EIS footer                                                                                                                                                              | Any                                                             |
| cssp_notification_signature_hris                 | Generates the HRIS Signature                                                                                                                                                          | Any                                                             |
| cssp_notification_mme_copy_manager               | Doesn't generate any content. It only copies the approving manager in the email. Can be included anywhere in the template.                                                            | sc_req_item                                                     |
| cssp_notification_inbound_error_response         | Prints the response if there is an error in the inbound action processing for HR Case                                                                                                 | Any                                                             |
| cssp_notification_record_link_from_approval      | Generates the record link the approval is for                                                                                                                                         | sysapproval_approver                                            |
| cssp_notification_record_link                    | Generates the record link for the portal                                                                                                                                              | sn_hr_core_case and extended, sc_request, sc_req_item, incident |
| cssp_hr_case_requisition_details                 | Prints the requisition details of the case to the approver                                                                                                                            | sn_hr_core_case_talent_management                               |
| cssp_notification_attachment_section_header      | Header for the attachment section                                                                                                                                                     | Any                                                             |
| cssp_hr_case_approval_history                    | Prints the approval history of the requisition case                                                                                                                                   | sn_hr_core_case_talent_management                               |
| cssp_notification_add_opened_by_hr               | Copies the opened by user to the email. Does NOT generate any content so it can be included anywhere in the template                                                                  | Any                                                             |
| cssp_notification_comments                       | Prints the latest 3 comments of the record. Doesn't include header                                                                                                                    | Any table with journal field                                    |
| cssp_notification_sc_request_contact_details     | Prints the contact detail based on user's preferred method of contact                                                                                                                 | sc_request                                                      |
| cssp_notification_sc_req_item_contact_details    | Prints the contact detail based on user's preferred method of contact                                                                                                                 | sc_req_item                                                     |
| cssp_notification_sc_task_contact_details        | Prints the contact detail based on user's preferred method of contact                                                                                                                 | sc_task                                                         |
| cssp_common_footer_hr                            | footer for HR                                                                                                                                                                         | Any                                                             |
| cssp_notification_approval_portal_link           | Generates the portal link to the approval record                                                                                                                                      | sysapproval_approver                                            |
| cssp_notification_request_item_details           | Prints the details of the request for the approval email. Formatted as just text                                                                                                      | sysapproval_approver                                            |
| cssp_hr_attach_employment_verification_letter    | prints attachments to the Employment Verification Case                                                                                                                                | sn_hr_core_case_workforce_admin                                 |
| cssp_notification_worknotes_section_header       | Prints the worknotes header                                                                                                                                                           | any                                                             |
| cssp_notification_reject_completion_link         | Generates link to reject case closure                                                                                                                                                 | sn_hr_core_case and extended                                    |
| cssp_notification_last_comment_eis               | Prints the latest comment                                                                                                                                                             | Any                                                             |
| cssp_notification_action_required_section_header | Prints the action header                                                                                                                                                              | Any                                                             |
| cssp_notification_worknotes                      | Prints the latest 3 worknotes for the record. Doesn't include header                                                                                                                  | Any table with journal field                                    |
| cssp_notification_signature_change               | Prints the change team's signature                                                                                                                                                    | Any                                                             |
| cssp_kb_activity_description                     | Prints the knowledge article fields                                                                                                                                                   | sn_actsub_activity                                              |
| cssp_kb_activity_link                            | Prints the link to the knowledge article                                                                                                                                              | sn_actsub_activity                                              |
| cssp_notification_last_comment                   | Prints the last comment for the record                                                                                                                                                | Any table with journal field                                    |
| cssp_notification_record_link_hr                 | Prints the portal link for the hr case                                                                                                                                                | sn_hr_core_case and extended                                    |
| cssp_notification_mdn_subject                    | Prints the subject line for escalation emails based on if user if part of MDN or not                                                                                                  | sn_hr_core_case and extended                                    |
| cssp_notification_last_worknote                  | Prints the last worknote for the record                                                                                                                                               | Any table with journal field                                    |
| cssp_notification_record_link_from_survey        | Prints the portal record link for the survey email                                                                                                                                    | asmt_assessment_instance                                        |
| cssp_notification_survey_link                    | prints the portal survey link                                                                                                                                                         | sc_req_item, sn_hr_core_case and extended                       |
| cssp_notification_signature_hr                   | Prints the HR Signature                                                                                                                                                               | Any                                                             |
| cssp_notification_request_details                | Prints the number, item, requested for and requested by for the RITM                                                                                                                  | sc_req_item                                                     |
| cssp_notification_comments_section_header        | Header for the comments section                                                                                                                                                       | Any                                                             |
| cssp_notification_action_section_header          | Header for the action section                                                                                                                                                         | Any                                                             |
| cssp_notification_information_section_header     | Header for the information section                                                                                                                                                    | Any                                                             |
| cssp_notification_signature_eis                  | EIS Signature                                                                                                                                                                         | Any                                                             |

## General / Miscellaneous Tips

- Sometimes when an email doesn't send, if could be one of these issues
  - Email not turned on in sub-production instance. Each clone resets this option and needs to be turned on again
  - The recipient doesn't have an email listed in the user record
  - There is an issue with the mail script syntax: check with "Preview Notification" and if it shows up blank then there is a syntax issue
  - Send to event creator not checked and the notification is only sent to the event creator
- Send to event creator
  - If not checked, this gathers up all the recipient field of the notificationa and organizest them into an array, and at the very **END**, it'll remove the person who triggered the notification from the list (condition builder included)
- Be careful of what styles you put in the notifcation. ServiceNow will automatically remove some styles, and then each mail client may remove styles as well. Always test on multiple platforms (including mobile)
