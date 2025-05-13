# ClickUpAPI Documentation

## Overview
The `ClickUpAPI` class provides methods to interact with the ClickUp API, including creating, editing, and deleting tasks, as well as handling custom fields, assignees, and attachments.

---

## Constructor

### `constructor(apiKey)`
Initializes the `ClickUpAPI` class.

- **Parameters**:
  - `apiKey` (string): The API key for authenticating with the ClickUp API.
- **Throws**:
  - Error if the `apiKey` is not provided.

---

## Methods

### `postTask(body, listId, teamId, taskId = null)`
Creates or updates a task in ClickUp.
Note: listId and teamId can be found in the lists url. `https://app.clickup.com/{TEAM_ID}/v/li/{LIST_ID}`

- **Parameters**:
  - **`listId` (string):** The listId to put the task in
  - **`teamId` (string):** The teamId of the space the list is in
  - **`taskId` (string):** if provided, id of task to update
  - **`body` (object):** The task details.
    - **`name` (string, required):** The name of the task.
    - **`description` (string):** The description of the task.
    - **`assignees` (array of integers):** List of assignee user IDs.
    - **`archived` (boolean):** Indicates if the task is archived.
    - **`group_assignees` (array of strings):** Assign multiple user groups to the task.
    - **`tags` (array of strings):** Tags associated with the task.
    - **`status` (string):** The status of the task.
    - **`priority` (integer | null):** The priority level of the task.
    - **`due_date` (integer):** The due date of the task in milliseconds.
    - **`due_date_time` (boolean):** Indicates if the due date includes a specific time.
    - **`time_estimate` (integer):** Estimated time to complete the task in milliseconds.
    - **`start_date` (integer):** The start date of the task in milliseconds.
    - **`start_date_time` (boolean):** Indicates if the start date includes a specific time.
    - **`points` (number):** Sprint points assigned to the task.
    - **`notify_all` (boolean):** If true, notifications will be sent to everyone including the creator of the comment.
    - **`parent` (string | null):** The ID of the parent task to create a subtask. The parent task must be in the same list specified in the path parameter.
    - **`markdown_content` (string):** Markdown formatted description for the task. If both `markdown_content` and `description` are provided, `markdown_content` will be used.
    - **`links_to` (string | null):** Include a task ID to create a linked dependency with your new task.
    - **`check_required_custom_fields` (boolean):** Enforce required custom fields by setting this to true. Default is false.
    - **`custom_fields` (array of objects):** Custom fields for the task.
    - **`custom_item_id` (number):** The custom task type ID for this task. A value of null creates a standard task type "Task".

---

### `deleteTask(taskId)`

Deletes a task in ClickUp.

- **Parameters**:
  - `taskId` (string): The ID of the task to delete (required).
- **Returns**:
  - An object containing the HTTP status code of the deletion request.
- **Throws**:
  - Error if the `taskId` is not provided or the API request fails.

---

### `convertCustomFields(customFields, listId)`

Maps custom fields to their corresponding IDs in the specified list.

- **Parameters**:
  - `customFields` (array): Array of custom field objects (`{ name, value }`).
  - `listId` (string): The ID of the ClickUp list.
- **Returns**:
  - An array of mapped custom fields with their IDs and values.

---

### `convertAssignees(assignees, listId)`

Maps assignee emails to their corresponding user IDs in the specified list.

- **Parameters**:
  - `assignees` (array): Array of assignee emails.
  - `listId` (string): The ID of the ClickUp list.
- **Returns**:
  - An array of mapped assignee user IDs.

---

### `addAttachments(taskId, attachments)`

Uploads attachments to a task.

- **Parameters**:
  - `taskId` (string): The ID of the task to add attachments to.
  - `attachments` (array): Array of attachment objects (`{ fileName, url }`).
- **Returns**:
  - An array of attachment IDs.

---

### `getCustomFieldsInList(listId)`

Fetches the custom fields available in a specific list.

- **Parameters**:
  - `listId` (string): The ID of the ClickUp list.
- **Returns**:
  - An array of custom fields available in the list.
- **Throws**:
  - Error if the API request fails.

---

### `getUsersInList(listId)`

Fetches the users available in a specific list.

- **Parameters**:
  - `listId` (string): The ID of the ClickUp list.
- **Returns**:
  - An array of users available in the list.
- **Throws**:
  - Error if the API request fails.

---

### `postAttachment(taskId, attachment)`

Uploads a single attachment to a task.

- **Parameters**:
  - `taskId` (string): The ID of the task to add the attachment to.
  - `attachment` (object): The attachment object (`{ fileName, url }`).
- **Returns**:
  - The uploaded attachment object.
- **Throws**:
  - Error if the API request fails.

---

### `createRequestOptions(method, body = null)`

Creates the request options for an API call.

- **Parameters**:
  - `method` (string): The HTTP method (e.g., `GET`, `POST`, `PUT`, `DELETE`).
  - `body` (object, optional): The request body.
- **Returns**:
  - An object containing the request options.

---

### `validateTaskInput(body, listId, teamId)`

Validates the input for creating or updating a task.

- **Parameters**:
  - `body` (object): The task details.
  - `listId` (string): The ID of the ClickUp list.
  - `teamId` (string): The ID of the ClickUp team.
- **Throws**:
  - Error if any required parameter is missing.

---

## Example Usage

```javascript
import ClickUpAPI from './ClickUpAPI.js';

const apiKey = process.env.CLICKUP_API_KEY;
const clickUp = new ClickUpAPI(apiKey);

const taskBody = {
  name: "New Task",
  description: "Task description",
  assignees: ["user@example.com"],
  custom_fields: [{ name: "Priority", value: "High" }],
};

const listId = "12345";
const teamId = "67890";

clickUp.postTask(taskBody, listId, teamId)
  .then(response => console.log("Task created:", response))
  .catch(error => console.error("Error:", error));
```