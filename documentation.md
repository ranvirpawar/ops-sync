# 🌟 Hierarchical Ticketing System: Project Documentation 🌟

Welcome to the ultimate guide for building a **two-level hierarchical ticketing system**! This document is your roadmap to creating a robust, user-friendly system using **Flutter** and **Firebase**. Packed with features like employee authentication, ticket management, task tracking, and role-based personalization, this project is designed to streamline workflows and boost productivity.

---

## 🚀 Project Scope and Overview

This documentation provides a comprehensive guide for developing a two-level hierarchical ticketing system with the following **core components**:

- **Employee Authentication**: Secure login and role-based access control to keep the system safe and organized.
- **Ticket Management**: Creation, tracking, and resolution of tickets to manage issues efficiently.
- **Task Management**: Granular task tracking within tickets for detailed progress monitoring.
- **User Role Personalization**: Tailored views and permissions based on user roles for a personalized experience.

The system will be built using **Flutter** for seamless cross-platform support (iOS, Android, and web) and **Firebase** as the powerhouse backend infrastructure.

---

## ⚙️ System Requirements

### 🎯 Functional Requirements

#### 👤 User Management
- User registration and authentication via **Firebase Auth**.
- Role-based access: **Admin**, **Manager**, **Employee**.
- Profile management with personal details.
- Department assignment for organizational structure.

#### 🎫 Ticket Management
- Create, view, and edit tickets *(no deletion allowed)*.
- Ticket types: **Bug**, **Feature Request**, **Support**, **Maintenance**.
- Status workflow: **New → In Progress → On Hold → Resolved → Closed**.
- Priority levels: **Low**, **Medium**, **High**, **Critical**.
- Department/category assignment for smart routing.
- Due date tracking and SLA monitoring.
- Project grouping capability.
- Ticket assignment to specific users.

#### ✅ Task Management
- Tasks linked to parent tickets.
- Task types: **Development**, **Testing**, **Documentation**, **Research**.
- Task status workflow: **Todo → In Progress → Review → Done**.
- User assignment for accountability.
- Due date tracking for timely completion.
- Dependency tracking between related tasks.
- Subtask support for complex breakdowns.

#### 🔔 Notification System
- In-app notifications for real-time updates.
- Email notifications for critical changes.
- Push notifications via **Firebase Cloud Messaging**.

#### 📊 Reporting & Analytics
- User performance metrics.
- Ticket resolution time tracking.
- Status transition analytics.
- Daily statistical reporting.

### 🛠️ Non-Functional Requirements
- **Performance**: Lightning-fast response times (< 2 seconds) for all operations.
- **Scalability**: Ready to handle a growing user base and ticket volume.
- **Reliability**: 99.9% uptime for critical functions.
- **Security**: Role-based access control and data encryption.
- **Usability**: Intuitive UI with responsive design across all devices.
- **Offline Support**: Basic functionality when the network drops.

---

## 🏗️ Technical Architecture

### 🛠️ Technology Stack
- **Frontend**: **Flutter** (cross-platform magic for iOS, Android, and web).
- **Backend**: **Firebase**
  - **Cloud Functions**: Serverless backend logic.
  - **Firestore**: NoSQL database for flexible storage.
  - **Authentication**: Secure user management.
  - **Cloud Messaging**: Push notifications.
  - **Storage**: File attachments.

### 📈 Flowchart
Here’s a visual representation of the system architecture using Mermaid:

```mermaid
flowchart TD
    subgraph Client
        U[User] --> M[Mobile App]
        U --> W[Web App]
    end
    
    subgraph Firebase
        Auth[Authentication]
        FS[Firestore Database]
        FCM[Cloud Messaging]
        CF[Cloud Functions]
        S[Storage]
    end
    
    M <--> Auth
    W <--> Auth
    M <--> FS
    W <--> FS
    M <--> FCM
    W <--> FCM
    M <--> S
    W <--> S
    
    FS <--> CF
    Auth <--> CF
    FCM <--> CF
    
    subgraph External
        E[Email Service]
    end
    
    CF <--> E
    🌟 Hierarchical Ticketing System Documentation
Welcome to the comprehensive guide for developing a robust, user-friendly Hierarchical Ticketing System! This system leverages Flutter for cross-platform awesomeness and Firebase for a scalable backend. With features like user authentication, ticket and task management, and role-based access, we’re building something epic. Let’s dive in! 🚀

🎯 Project Overview
This ticketing system is designed to streamline task management within projects. It’s hierarchical—tickets group tasks, and projects group tickets. Key features include:

User Authentication: Secure login with role-based access (admin, manager, employee).
Ticket Management: Create, assign, and track tickets with statuses and priorities.
Task Management: Nested tasks with dependencies and subtasks.
Notifications: Keep users in the loop with in-app, email, and push alerts.
Projects: Organize tickets under projects with team member management.
Built with Flutter for a seamless UI across platforms and Firebase for authentication, Firestore database, and Cloud Functions, this system is both powerful and flexible.

💾 Database Schema
📋 Firestore Collections Structure
Here’s how our data is organized in Firestore, a NoSQL database perfect for this hierarchical setup. Below is an Entity-Relationship (ER) Diagram crafted with Mermaid to visualize the connections:

mermaid

Collapse

Wrap

Copy
erDiagram
    Users ||--o{ Tickets : creates
    Users ||--o{ Tasks : assigned
    Users ||--o{ Notifications : receives
    Tickets ||--o{ Tasks : contains
    Tickets ||--o{ Comments : has
    Tasks ||--o{ Comments : has
    Projects ||--o{ Tickets : groups
    
    Users {
        string uid PK
        string email
        string displayName
        string role
        string department
        timestamp createdAt
        timestamp lastActive
        string fcmToken
    }
    
    Tickets {
        string id PK
        string title
        string description
        string type
        string status
        string priority
        string createdBy FK
        string assignedTo FK
        string department
        string project FK
        timestamp createdAt
        timestamp updatedAt
        string lastUpdatedBy FK
        timestamp dueDate
        array tags
        array watchers
    }
    
    Tasks {
        string id PK
        string ticketId FK
        string title
        string description
        string type
        string status
        string assignedTo FK
        string createdBy FK
        timestamp createdAt
        timestamp updatedAt
        string lastUpdatedBy FK
        timestamp dueDate
        int order
        array dependencies
        boolean isSubtask
        string parentTaskId FK
    }
    
    Comments {
        string id PK
        string parentId FK
        string text
        string createdBy FK
        timestamp createdAt
        timestamp updatedAt
        boolean isEdited
    }
    
    Notifications {
        string id PK
        string userId FK
        string title
        string message
        string relatedTo
        string relatedToType
        string type
        boolean read
        timestamp createdAt
    }
    
    ActivityLogs {
        string id PK
        string action
        string entityId
        string entityType
        string userId FK
        timestamp timestamp
        object details
    }
    
    Projects {
        string id PK
        string name
        string description
        string department
        timestamp startDate
        timestamp endDate
        string status
        array members
    }
Note: If your Markdown viewer doesn’t support Mermaid, use the Mermaid Live Editor to generate a PNG image and embed it like this:

markdown

Collapse

Wrap

Copy
![ER Diagram](./path-to-er-diagram.png)
🔍 Detailed Collection Descriptions
👤 Users Collection
text

Collapse

Wrap

Copy
users/{userId}
  - uid: string (primary key)
  - email: string
  - displayName: string
  - role: string (admin, manager, employee)
  - department: string
  - createdAt: timestamp
  - lastActive: timestamp
  - fcmToken: string (optional, for push notifications)
🎫 Tickets Collection
text

Collapse

Wrap

Copy
tickets/{ticketId}
  - title: string
  - description: string
  - type: string (bug, feature, support, maintenance)
  - status: string (new, inProgress, onHold, resolved, closed)
  - priority: string (low, medium, high, critical)
  - createdBy: string (userId)
  - assignedTo: string (userId)
  - department: string
  - project: string (projectId)
  - createdAt: timestamp
  - updatedAt: timestamp
  - lastUpdatedBy: string (userId)
  - dueDate: timestamp
  - tags: array of strings
  - watchers: array of userIds
✅ Tasks Collection
text

Collapse

Wrap

Copy
tickets/{ticketId}/tasks/{taskId}
  - title: string
  - description: string
  - type: string (development, testing, documentation, research)
  - status: string (todo, inProgress, review, done)
  - assignedTo: string (userId)
  - createdBy: string (userId)
  - createdAt: timestamp
  - updatedAt: timestamp
  - lastUpdatedBy: string (userId)
  - dueDate: timestamp
  - order: number (for sorting)
  - dependencies: array of taskIds
  - isSubtask: boolean
  - parentTaskId: string (taskId, if isSubtask is true)
💬 Comments Collection
text

Collapse

Wrap

Copy
tickets/{ticketId}/comments/{commentId}
tickets/{ticketId}/tasks/{taskId}/comments/{commentId}
  - text: string
  - createdBy: string (userId)
  - createdAt: timestamp
  - updatedAt: timestamp
  - isEdited: boolean
🔔 Notifications Collection
text

Collapse

Wrap

Copy
users/{userId}/notifications/{notificationId}
  - title: string
  - message: string
  - relatedTo: string (ticketId or taskId)
  - relatedToType: string (ticket or task)
  - type: string (assignment, mention, status_change, etc.)
  - read: boolean
  - createdAt: timestamp
📜 ActivityLogs Collection
text

Collapse

Wrap

Copy
activityLogs/{logId}
  - action: string
  - entityId: string (ticketId or taskId)
  - entityType: string (ticket or task)
  - userId: string
  - timestamp: timestamp
  - details: map
📋 Projects Collection
text

Collapse

Wrap

Copy
projects/{projectId}
  - name: string
  - description: string
  - department: string
  - startDate: timestamp
  - endDate: timestamp
  - status: string
  - members: array of userIds
📅 Development Phases
🌱 Phase 1: Project Setup and Initial Configuration
Create Firebase Project
Set up a new Firebase project.
Configure authentication methods (email/password, etc.).
Initialize Firestore database.
Set up Cloud Functions.
Create Flutter Project
Initialize Flutter project.
Configure dependencies (Firebase, etc.).
Set up project structure.
Implement Firebase integration.
Implement Authentication
User registration flow.
Login functionality.
Role-based authorization.
Password reset functionality.
🌟 Phase 2: Core Functionality Development
User Management
Create user profile screens.
Implement role management.
Set up department assignments.
Profile editing functionality.
Ticket Management
Create ticket listing views.
Implement ticket creation forms.
Build ticket detail screens.
Implement status workflow.
Set up priority handling.
Develop assignment functionality.
Task Management
Create task board views (e.g., Kanban).
Implement task creation.
Set up status transitions.
Implement assignment functionality.
Build dependency tracking.
Set up subtask functionality.
🚀 Phase 3: Advanced Features
Notification System
Set up in-app notifications.
Implement email notifications.
Configure push notifications via FCM.
Create notification preferences.
Reporting & Analytics
Build dashboard views.
Implement metrics calculations.
Create report generation.
Set up export functionality (PDF, CSV).
Project Management
Create project creation interface.
Implement project-based views.
Set up project member management.
Configure ticket grouping.
✅ Phase 4: Testing and Deployment
Unit Testing
Test individual components.
Validate business logic.
Test authentication flow.
Integration Testing
Test component interactions.
Validate workflow processes.
Test Firebase integration.
User Acceptance Testing
Gather feedback from stakeholders.
Refine UI/UX based on feedback.
Fix identified issues.
Deployment
Prepare production environment.
Deploy to app stores (iOS/Android).
Configure web deployment.
Set up monitoring (e.g., Crashlytics).
🎨 Frontend Implementation Details
📂 Project Structure
text

Collapse

Wrap

Copy
lib/
  ├── main.dart              # App entry point
  ├── app.dart              # App configuration
  ├── config/
  │   ├── routes.dart       # Navigation routes
  │   ├── themes.dart       # UI themes
  │   └── constants.dart    # App-wide constants
  ├── models/
  │   ├── user.dart         # User data model
  │   ├── ticket.dart       # Ticket data model
  │   ├── task.dart         # Task data model
  │   └── project.dart      # Project data model
  ├── services/
  │   ├── auth_service.dart # Firebase auth logic
  │   ├── ticket_service.dart # Ticket CRUD
  │   ├── task_service.dart # Task CRUD
  │   └── notification_service.dart # Notification handling
  ├── providers/
  │   ├── auth_provider.dart # State management for auth
  │   ├── ticket_provider.dart # Ticket state
  │   └── task_provider.dart # Task state
  ├── screens/
  │   ├── auth/             # Login, register screens
  │   ├── dashboard/        # Main dashboard
  │   ├── tickets/          # Ticket management screens
  │   ├── tasks/            # Task management screens
  │   └── settings/         # User settings
  └── widgets/
      ├── common/           # Reusable UI components
      ├── ticket/           # Ticket-specific widgets
      └── task/             # Task-specific widgets
🖼️ Key UI Components
Authentication Screens
Login screen with email/password.
Registration screen for new users.
Password reset screen.
Profile setup screen post-registration.
Dashboard
Overview stats (tickets, tasks).
Recent activities feed.
Quick action buttons.
Role-based personalized views.
Ticket Management
Filterable ticket list.
Ticket creation form.
Detailed ticket view with comments.
Status workflow buttons.
Task Management
Kanban board for task statuses.
Task creation form.
Task detail view with dependencies.
Subtask management UI.
🔧 Backend Implementation Details
🔥 Firebase Cloud Functions
Task Assignment Notification
Triggers on assignedTo field change.
Creates a notification in notifications.
Sends a push via FCM if token exists.
Ticket Status Change Tracking
Triggers on ticket status updates.
Logs to activityLogs.
Notifies creator and watchers.
Automatic Ticket Status Updates
Updates ticket status based on task completion.
Sets to Resolved when all tasks are Done.
Sets to In Progress when first task starts.
Daily Statistics Generation
Runs every 24 hours.
Calculates ticket/task stats.
Stores data for dashboard reporting.
🔒 Security Rules
text

Collapse

Wrap

Copy
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can read their own data
    match /users/{userId} {
      allow read: if request.auth.uid == userId;
      allow write: if request.auth.uid == userId && request.resource.data.role == resource.data.role;
    }
    
    // Ticket access rules
    match /tickets/{ticketId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update: if request.auth != null && 
                     (resource.data.createdBy == request.auth.uid || 
                      resource.data.assignedTo == request.auth.uid ||
                      get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin' ||
                      get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'manager');
    }
    
    // Tasks inside tickets
    match /tickets/{ticketId}/tasks/{taskId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && exists(/databases/$(database)/documents/tickets/$(ticketId));
    }
    
    // Comments
    match /tickets/{ticketId}/comments/{commentId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null && resource.data.createdBy == request.auth.uid;
    }
    
    match /tickets/{ticketId}/tasks/{taskId}/comments/{commentId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null && resource.data.createdBy == request.auth.uid;
    }
  }
}