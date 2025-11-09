# Vacation Tracking System
Our moto is: 
> The system must be easy to use.

# Vision
A Vacation Tracking System (VTS) will provide individual employees with the
capability to manage their own vacation time, sick leave, and personal time off,
without having to be an expert in company policy or the local facility’s leave
policies

# Problem Definition (Domain)
The system operates within the Human Resources (HR) domain, focusing on the management of employee leave requests.
Currently, employees submit leave requests manually, which leads to inconsistencies, processing delays, and limited visibility for both staff and managers.
The goal is to improve the organization’s internal HR processes by automating leave management, enforcing company policies, and maintaining accurate and accessible records of employee leave activities.

# Functional Requirements 
- Implements a flexible rules-based system for validating and verifying leave time requests
- Enables manager approval (optional)
- Provides access to requests for the previous calendar year, and allows requests to be made up to a year and a half in the future
- Uses e-mail notification to request manager approval and notify employees of request status changes
- Is implemented as an extension to the existing intranet portal system, and uses the portal’s single-sign-on mechanisms for all authentication - Keeps activity logs for all transactions
- Enables the HR and system administration personnel to override all actions restricted by rules, with logging of those overrides
- Allows managers to directly award personal leave time (with system-set limits)
- rovides a Web service interface for other internal systems to query any given employee’s vacation request summary
- Interfaces with the HR department legacy systems to retrieve required employee information and changes

# Non-functional Requirements 
- Uses existing hardware and middleware

# Constraints
- The system must use the existing intranet portal and single sign-on (SSO) system
- Uses existing hardware and middleware
- Only HR and managers can approve or override requests
- Must comply with company HR policies and data protection standards

# Assumptions
- All employees and managers have valid accounts in the intranet portal
- The company’s leave policies are already defined and documented
- HR policies regarding leave approval will remain stable during development

# Use case model
## System's actors:
- **Employee**: The main user of this system. An employee uses this system to
manage his or her vacation time
- **Manager**: An employee who has all the abilities and goals of a regular
employee, but with the added responsibility of approving vacation requests
for immediate subordinates. A manager may award subordinates comp
time, subject to certain limits set in the system
- **Clerk**: A member of the HR department who has sufficient rights to view
employees’ personal data and is responsible for ensuring that employees’
information in all HR systems is up to date and correct. An HR clerk can
add or remove nearly any record in the system. In the real world, HR clerks
may or may not be employees; however, if they are employees, they use two
separate login IDs to manage these two different roles
- **System Admin**: A role responsible for the smooth running of the system’s technical resources (e.g., Web server, database) and for collecting and
archiving all log files

## Main use cases:
- **Manage Time**: Describes how employees request and view vacation time
requests.
- **Approve Request**: Describes how a manager responds to a subordi-
nate’s request for vacation time
- **Award Time**: Describes how a manager can award a subordinate extra
leave time (comp time)
- **Edit Employee Record**: Describes how an HR clerk edits an employee’s
information in the system. This includes setting all the leave time allow-
ances and the maximum time that can be awarded by the manager
- **Manage Locations**: Describes how an HR clerk manages location
records and their rules
- **Manage Leave Categories**: Describes how an HR clerk manages
leave categories and their rules
- **Override Leave Records**: Describes how an HR clerk may override
any rejection of leave time requests made by the rules in the system.
- **Back Up System Logs**: Describes how the system administrator backs
up the system’s logs

Let's focus on "Manage Time" use case, this use case by far is the most frequently invoked and the one most
viewed by all the actors of the system. As a result, it is critical to implement this
use case effectively and to ensure that it meets all of the overall design goals,
including the ease-of-use feature.

### Manage Time Use case
#### Main flow:
- **Actors**: Employee and Manager
- **Goal**: The employee wishes to submit a new request for vacation time
- **Preconditions**: The employee is authenticated by the portal framework and
identified as an employee of the company with privileges to manage his or her
own vacation time

Following is the flowchart represesnting the above use case in two spearate lanes: 
one for the employee submitting a request and another one for a manager approving or rejecting a request

<img width="3831" height="3416" alt="manage-time-use-case" src="https://github.com/user-attachments/assets/02998254-e5f3-478b-a324-d347f9b1deac" />


Following is the ER digram for the same use case:

![Manage Time use case ER digram](https://github.com/user-attachments/assets/9d99a093-7b6e-4cb6-a97a-a7f1f142c8ad)


Following is the sequence digram of an employee submitting a request: 

<img width="2320" height="2138" alt="Manage Time use case Sequence diagram-7" src="https://github.com/user-attachments/assets/a53e1b9c-7998-4671-820e-8372c52dce69" />


Following is the sequence digram of a manager approving a request:

<img width="1880" height="2078" alt="Manage Time use case Sequence diagram-10" src="https://github.com/user-attachments/assets/79c47ffa-de2e-4f58-8021-8d2a98a9366c" />

Pseudocode:
- Authentication Flow
```
function authenticateEmployee(credentials)
    IF not isValidCredentials(credentials) THEN
        return Error("Access denied")
    END IF

    employee = findEmployeeByCredentials(credentials)
    return employee
end function
```

- Vacation Request Submission
```
function submitVacationRequest(employee, requestData)
    IF not isRequestDataComplete(requestData) THEN
        return Error("Incomplete request information")
    END IF

    category = getVacationCategory(requestData.categoryId)
    IF category is null THEN
        return Error("Invalid vacation category")
    END IF

    IF not hasSufficientBalance(employee, category, requestData) THEN
        return Error("Insufficient vacation balance")
    END IF

    request = createVacationRequest(employee, category, requestData)
    validateVacationRequest(request)

    saveVacationRequest(request)

    IF request.requiresApproval THEN
        notifyManager(request)
        updateRequestStatus(request, "Pending Approval")
    ELSE
        updateRequestStatus(request, "Approved")
    END IF

    return request
end function
```

- Manager Approval
```
function processManagerApproval(manager)
    pendingRequests = getPendingRequests(manager)

    FOR each request IN pendingRequests DO
        decision = getManagerDecision(request)
        IF decision = "approve" THEN
            approveRequest(request)
            notifyEmployee(request.employee, "approved")
        ELSE
            rejectRequest(request, decision.reason)
            notifyEmployee(request.employee, "rejected", decision.reason)
        END IF
    END FOR
end function
```

- Notification Handling
```
function notifyManager(request)
    sendEmail(to=request.manager.email, subject="Vacation Request Pending", body=request.details)
end function

function notifyEmployee(employee, status, reason=null)
    message = buildNotificationMessage(status, reason)
    sendEmail(to=employee.email, subject="Vacation Request " + status, body=message)
end function
```

#### Alternate flow: Withdraw Request
- **Actor**: Employee
- **Goal**: The employee wants to withdraw an outstanding request for vacation time
- **Preconditions**: An employee has made a vacation time request, and that
request has yet to be approved or denied by an authorized manager. See also
main flow preconditions

Following is the flowchart of the withdraw alternate flow:
<img width="3098" height="5890" alt="Untitled-2025-11-01-1346-2" src="https://github.com/user-attachments/assets/53a1729d-da9d-4717-91e5-00023313b2c5" />

Following is the sequence digram withdraw alternate flow:








