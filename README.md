# Vacation Tracking System (VTS)

## 📘 Overview

This project is a web-based **Vacation Tracking System (VTS)** designed to streamline how employees request time off and how managers/HR handle approvals. It aims to:

- Empower employees to manage their own time.
- Reduce delays and overhead in HR operations.
- Automate request validation using rules.
- Provide a user-friendly and intuitive interface.

---

## 🎯 Key Features

- Submit, edit, or withdraw vacation requests.
- Automatic validation based on company rules.
- Optional manager approval via email.
- Visual calendar for date selection.
- Admin override capability (logged).
- Integration with existing HR systems.
- Access to past and future requests (±18 months).
- Notification system for both employees and managers.

---

## 🧑‍🤝‍🧑 Actors

- **Employee**: Main user who submits and manages vacation requests.
- **Manager**: Approves or denies employee requests; can grant leave.
- **HR Personnel**: Edits employee records, manages policies and overrides.
- **System Admin**: Maintains technical system health and logs.

---

## ✅ Functional Requirements

- **Manage Time**: Submit new vacation request.
- **Edit Request**: Update pending vacation requests.
- **Withdraw Request**: Remove a submitted request.
- **Cancel Approved**: Cancel approved but unused request.

---

## 🧭 Use Case Diagram
![Use Case Diagram](https://github.com/user-attachments/assets/1d13ee84-391d-4237-92d3-54d9d7ca820c)

---

## 🔁 Sequence Diagram: Manage Time Request
![Sequence Diagram](https://github.com/user-attachments/assets/ca15eab6-ec63-4249-ae9e-70a71f080f6a)

---
### 🧾 Flowchart: Vacation Request Submission
![Request Flowchart](https://github.com/aboagwa/Vacation-Tracking-System/blob/main/Flow-Chart.png)
---
## 📊 Entity-Relationship Diagram (ERD)

![ERD](![ERD drawio](https://github.com/user-attachments/assets/f1f3d879-607b-48d0-8993-f91b280553d1)) 
## ✍️ Pseudocode

```pseudocode
FUNCTION handleManageTimeRequest(action, requestData):

  IF action == "VIEW_HOME":
    pending = getPendingRequests(employeeId)
    approved = getApprovedRequests(employeeId)
    balances = getLeaveBalances(employeeId)
    RETURN showHomePage(pending, approved, balances)

  ELSE IF action == "VIEW_REQUEST":
    request = getRequestById(requestData.id)
    IF request belongs to employeeId:
      RETURN showRequestDetails(request)
    ELSE:
      RETURN showError("Access Denied")

  ELSE IF action == "SHOW_CREATE_FORM":
    RETURN showCreateForm()

  ELSE IF action == "CREATE_REQUEST":
    newRequest = createNewRequest(employeeId, requestData.formData)
    isValid, errors = validateRequest(newRequest)

    IF isValid:
      newRequest.status = "Pending"
      savedRequest = saveRequest(newRequest)

      IF needsManagerApproval(savedRequest):
        notifyManager(savedRequest.managerId, savedRequest.id)

      notifyEmployee(employeeId, "Request Created", savedRequest.id)
      RETURN goToHome("Request successfully created.")
    ELSE:
      RETURN showCreateForm(requestData.formData, errors)

  ELSE IF action == "SHOW_EDIT_FORM":
    request = getRequestById(requestData.id)
    IF request belongs to employeeId AND request.status == "Pending":
      RETURN showEditForm(request)
    ELSE:
      RETURN showError("Cannot edit this request.")

  ELSE IF action == "UPDATE_REQUEST":
    request = getRequestById(requestData.id)
    IF NOT (request belongs to employeeId AND request.status == "Pending"):
      RETURN showError("Cannot update this request.")

    updateRequest(request, requestData.formData)
    isValid, errors = validateRequest(request)

    IF isValid:
      saveRequest(request)
      notifyEmployee(employeeId, "Request Updated", request.id)
      RETURN goToHome("Request successfully updated.")
    ELSE:
      RETURN showEditForm(request, errors)

  ELSE IF action == "CONFIRM_WITHDRAW":
    request = getRequestById(requestData.id)
    IF request belongs to employeeId AND request.status == "Pending":
      RETURN showWithdrawConfirmation(request)
    ELSE:
      RETURN showError("Cannot withdraw this request.")

  ELSE IF action == "WITHDRAW_REQUEST":
    request = getRequestById(requestData.id)
    IF request belongs to employeeId AND request.status == "Pending":
      request.status = "Withdrawn"
      saveRequest(request)
      notifyEmployee(employeeId, "Request Withdrawn", request.id)
      RETURN goToHome("Request successfully withdrawn.")
    ELSE:
      RETURN showError("Cannot withdraw this request.")

  ELSE:
    RETURN showError("Invalid action.")
END FUNCTION

---

## 📚 Reference

Based on concepts from:

**Object-Oriented Analysis and Design with Applications (3rd Edition)**  
by Grady Booch  
ISBN: 978-0201895513

---
