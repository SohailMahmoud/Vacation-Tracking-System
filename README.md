# Vacation-Tracking-System
Our moto is: the system must be easy to use.

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
