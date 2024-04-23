# API Sample Test

## Getting Started

This project requires a newer version of Node. Don't forget to install the NPM packages afterwards.

You should change the name of the ```.env.example``` file to ```.env```.

Run ```node app.js``` to get things started. Hopefully the project should start without any errors.

## Explanations

The actual task will be explained separately.

This is a very simple project that pulls data from HubSpot's CRM API. It pulls and processes company and contact data from HubSpot but does not insert it into the database.

In HubSpot, contacts can be part of companies. HubSpot calls this relationship an association. That is, a contact has an association with a company. We make a separate call when processing contacts to fetch this association data.

The Domain model is a record signifying a HockeyStack customer. You shouldn't worry about the actual implementation of it. The only important property is the ```hubspot```object in ```integrations```. This is how we know which HubSpot instance to connect to.

The implementation of the server and the ```server.js``` is not important for this project.

Every data source in this project was created for test purposes. If any request takes more than 5 seconds to execute, there is something wrong with the implementation.

## Improvements

1. I would suggest improving the code initially by using TypeScript everywhere and enforcing type-checking with ESLint validations. In this way, we can minimize errors. Also, I would suggest splitting the code that is in worker.js into separate service files, probably one for each separate processing task (e.g., ContactsService.ts, MeetingsService.ts, etc.). Each file will have smaller functions with defined functionality instead of one big function performing the entire processing (e.g., getNewlyModifiedMeetings(), getMeetingsAssociation(meetingIds), persistMeetings(...), processMeetings()).
2. For improving the project architecture, I would create separate folders, splitting the configurations from the code and then separating different modules/features inside subfolders. For example:
    - src/
      - features
        - hubspot
          - HubspotProcessor.ts (processMeetings(), processContacts(), processCompanies() etc)
          - MeetingsService.ts
          - ContactsService.ts
          - CompanyService.ts
        - utilities
          - utils.ts
      - app.ts
      - service.ts
    - package.json
3. For optimizing performance, we can have separate executions for each processor: one for Meetings, one for Contacts, one for Companies, etc. Every processor will be called individually at different times and periods in this way. We can even improve it further by using a queue for queuing different batch sizes so that we can process smaller chunks at a time. Another option that may be considered is using Hubspot webhooks in conjunction with the pulling mechanism. In this way, we can trigger processing when some quantity of new objects is created or modified.