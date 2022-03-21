# Trello-Item-Alert

Trello offers a notification system based on card due date and notification system documented at https://help.trello.com/article/793-receiving-trello-notifications .

The advanced checklist allows for item due date and assigning of card member on the item. However, there is no notification when the item is due or a reminder based on the due date.

This solution addresses the above by :

- create a alert record when there is a due date on an item.
- a cron job will process all items with an "incomplete" state and due date matching a pre-determined criterion.
- notify the user if a member is assigned to the card using **"@username some meaningful text" or "@card some meaningful text"** if no member is assigned to the item
- set a expiry on the alert record delete the record.
  - when notification has sent.
  - when item is marked "complete".
- when a new due date is set after due date had expires or when item state is changed after notification, the item will be eligible for notification again.

## Endpoints

### /itemUpdate

This serves as the webhook endpoint. The idModel is the board_id.

### /create

This is creates the alert record.

### /setup

This creates a webhook in Trello based on the idModel. This this case, it will be the board_id.

### /stop

This deletes the webhook in Trello by a board.

The endpoint documentation is available at https://5o5jrk.deta.dev/redoc .

## Pre-requisites

The pre-requisites includes :
- the admin Trello API Key and Token in the .env file.
- defined criterion to be used for the cron job.

## Technical Notes

The solution components include :

- A Deta Micro using FastAPI deployed on deta.dev or other platform like Wayscript X.
  - a cron to process and create the alerts.
- A Deta Base for the alert records.
- Trello webhook using the board id as the idModel.
- There is 10s timeout for all micro and cron for a Deta deployment. Request can be made to increase this.
