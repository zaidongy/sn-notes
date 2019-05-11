# ServiceNow Knowledge 2019 Notes

![Technical](https://placehold.it/15/f03c15/000000?text=+) `Technical`
![Conceptual](https://placehold.it/15/c5f015/000000?text=+) `Conceptual`

## ![Technical](https://placehold.it/18/f03c15/000000?text=+) Healthscan

- ServiceNow healthscan can assess instance health and upgradability.
- Review all configurations and customizations, can give us a holistic view of the instance deviant from OOO
- [Documentation](https://www.servicenow.com/content/dam/servicenow-assets/public/en-us/doc-type/data-sheet/ds-configuration-review.pdf)

## ![#Conceptual](https://placehold.it/15/c5f015/000000?text=+) Virtual Agent

- VA can automatically resolve most L1 incidents and cases
- VA can be applied to both IT and HR
- VA has the ability to transfer to a live agent, with all the required information collected
- VA can show business value
  - Track how many tickets it deflected (resolved without transferring to agent)
  - Calculate cost savings by mutiplying above # with cost per ticket.
  - Example: 500 incidents deflected per month at $10 per incident = \$5,000 saved per month = \$60,000 saved per year.

## ![Technical](https://placehold.it/18/f03c15/000000?text=+) Reporting on HR Cases

- Must map record producers variable to table fields in order to retain data and report
- Can create different views for each HR Service to show/hide variables
- Need to create naming convention for fields created on each HR COE Table

## ![#Conceptual](https://placehold.it/15/c5f015/000000?text=+) Unified Employee Service Center

- Single portal > multiple portals
  - Better UX, Better UI, easier to maintain, seemless to employees
- In the future, intranet could be replaced with Service Portal. If company compliance policies allows
- Use metrics for continuous improvement after deployment
- Hire technical writers to write **end user focused** KB Articles, articles should be:
  1. Concise
  2. Searchable
  3. Show empathy and humor
  4. Relevant - filter using knowledge blocks and user criteria
  5. Consider consolidating new information into existing articles instead of writing a new one
  6. Answer AAQs - Actually Asked Questions
  7. Diverse and inclusive
  8. Have a way to provide feedback. Example: if a user has to read more than 2 paragraphs, ask them to send notify the article owner to revise.

## ![Technical](https://placehold.it/18/f03c15/000000?text=+) Creator Studio for developers

- Available in Orlando release
- New IDE for developers with intellisense and source control
- For both builders (Low code) and developers

## ![Technical](https://placehold.it/18/f03c15/000000?text=+) ServiceNow Developer Blog Implementation

- `https://developer.servicenow.com/blog.do`
- `blog.do` is a processesor in the platform
- Uses VFS! Virtual File System, speaker said he will post the implementation on the SN store which is super useful because SN doesn't have a native file system
- Pipeline
  1. Write articles in markdown
  2. Compile articles into static html sites via hugo (node package)
  3. Deploy html articles to instance's VFS
  4. VFS will automatically serve the HTML to the processor

## ![#Conceptual](https://placehold.it/15/c5f015/000000?text=+) A couple of ideas that came to mind in the midst of all this inspiration

### Policy Module

- Policy module that links to workflows and be able to scan existing workflows tied to that policy to see if it's compliant
- Able to export policies into a visual format that allows for more transparency into organizational processes
- Useful for auditing and able to provide evidence when business owners complain the process is wrong

### Company internal community Q&A section in Service Portal

- Similar to Stackoverflow's implentation of Q&A
- Users can vote on questions and answers which will help with search
- Topics could be anything, but questions need to be tagged (HR, IT, Legal, etc...)

## ![#Conceptual](https://placehold.it/15/c5f015/000000?text=+) ITBM Demand Management

- Help assess and prioritize customer requests for products and services, there are 4 main principles
- **Visibility** - Establish a clear line of sight to all platform requests from the business
- **Control** - Make fact-based decisions about which projects and enhancements the platform team undertakes; Evaluate benefits, risks of not doing, and force rank requests
- **Alignment** - Group platform projects and enhancements by business objectives
- **Planning and velocity** - Look ahead to predict future demand while becoming more agile in address current demands
- Demand Management States: Draft -> Submitted -> Screening -> Qualified -> Approved

## ![Technical](https://placehold.it/18/f03c15/000000?text=+) Report and Dashboard Cleanups

- Reports Archive Solution

  1. Identify unused reports
  2. Rename reports with "# Archive" - Example: "# Archive Open Incidents"
  3. Archive unused reports
  4. Restore reports on demand manually if still being used

- Homepage Metrics Solution

  1. Filter data (syslog_transaction)
  2. Aggregate data (group by user, homepage and browser)
  3. PA Data collection on the transaction times
  4. Visualize data in a dashboard to see perfomance of each homepage

- Quality reports should have:
  1. Actional insights
  2. Performance
  3. Design
  4. Access
  5. Compliant with **naming convention**

## ![#Conceptual](https://placehold.it/15/c5f015/000000?text=+) Configuration vs Customization

- Every ServiceNow choice involves risk
- Debating Configuration vs Customzation is of limited use
- Use a Risk Management process to make better decisions
