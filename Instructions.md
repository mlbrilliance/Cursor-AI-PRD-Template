# Product Requirements Document (PRD)

## Project Overview

A proof-of-concept for the "Streamlined HSA/FSA/COBRA Administration" use case using CrewAI, while mocking data, systems, and incorporating HITL, Ethical, and RAG agents.
Project Goal: Demonstrate the potential of CrewAI to automate and improve HSA/FSA/COBRA administration using simulated data and systems.

## Additional Implementation Details:

Leverage environment-based configuration (e.g., .env files) to store keys/secrets, environment modes (dev, test, prod), and Supabase/Gemini/CrewAI credentials.
Ensure that each developer has a local .env with the necessary environment variables, but never commit it to version control.
Adopt a consistent folder naming convention (e.g., frontend, backend, agents, etc.) to improve maintainability.



## Core Functionalities

- Organize the frontend and backend code in their own separate folders for easy organization.
- Connect to LangSmith to track the LLM calls and usage.
- Use Gemini LLM for all LLM and RAG functions.
- Establish a robust logging mechanism to track LLM usage, including timestamps, employee IDs, relevant user input (sanitized), and agent outputs.
- Set up a monitoring and alert system to identify performance issues, usage spikes, or error spikes in real time.


### 1. Chat Window

- The chat window should be the main component of the application. It should run on localhost and a different port than the admin portal. The chat window will be developed using Next.js 14, Shadcn UI components, Tailwind CSS, and Lucide Icons.

#### 1.1. Chat Window Size

- **Popup** - as soon as I run npm run dev, the chat window should pop up. It should be a small chat window that is always on top of the other windows and can move around the screen, be maximized, minimized, and closed.
- **Chat space** - the chat window should have a chat space where the user can type in their messages.
- **Send button** - the chat window should have a send button that the user can click to send their message.
- **Clear button** - the chat window should have a clear button that the user can click to clear the chat space.
- **History** - the chat window should have a history button that the user can click to see the history of the chat.
- **Policy** - the chat window should have a policy button that the user can click to see the policy.
- Use a floating draggable React component (e.g., a custom hook or third-party library) to achieve the move-around and always-on-top behavior.
Include minimal transitions and animations for open/close/minimize to enhance user experience.

#### 1.2. Sesson authentication
- The chat window should have a session authentication system that allows the user to login and logout by supplying their employee_id.
- The session authentication system should be stored in the browser's local storage.
- The session authentication system should be able to handle multiple users logging in and out at the same time.
- The employee_id had been mocked in the Supabase mock_employees table.
- Upon login, the chat window should display the user's name and employee_id.
- Upon logout, the chat window should clear the chat space and the user's name and employee_id.
- The sesssion should remember the employee chat history and save it to the Supabase mock_chat_history table after the user logs out and the next time the user logs in, if the user requests to see the chat history, the chat window should display the chat history from the last session.


### 2. Mocking Data and Systems

Since we can't use real systems, we'll create mock data and simulate interactions with external systems. We'll use Python data structures (lists, dictionaries) and functions to represent these and I want to store this mock data in Supabase.


#### 2.1. Mocking Employee Data
Let's create tables in Supabase for the mock data and then we'll use the Supabase client to get the data. The table structure should be as follows:

mock_employees = [
    {
        "employee_id": "12345",
        "name": "Alice Smith",
        "dob": "1985-03-15",
        "email": "alice.smith@example.com",
        "hsa_eligible": True,
        "fsa_eligible": False,
        "cobra_status": "Not Applicable",
        "dependents": [
            {"name": "Bob Smith", "dob": "2010-06-20", "relationship": "Child"},
        ]
    },
    {
        "employee_id": "67890",
        "name": "Bob Johnson",
        "dob": "1992-11-01",
        "email": "bob.johnson@example.com",
        "hsa_eligible": False,
        "fsa_eligible": True,
        "cobra_status": "Eligible",
        "dependents": []
    },
    {
        "employee_id": "13579",
        "name": "Carol Williams",
        "dob": "1988-07-22", 
        "email": "carol.williams@example.com",
        "hsa_eligible": True,
        "fsa_eligible": True,
        "cobra_status": "Not Applicable",
        "dependents": [
            {"name": "David Williams", "dob": "2015-03-10", "relationship": "Child"},
            {"name": "Emma Williams", "dob": "2018-09-15", "relationship": "Child"}
        ]
    },
    {
        "employee_id": "24680", 
        "name": "Daniel Brown",
        "dob": "1979-12-05",
        "email": "daniel.brown@example.com",
        "hsa_eligible": False,
        "fsa_eligible": True,
        "cobra_status": "Not Eligible",
        "dependents": [
            {"name": "Sarah Brown", "dob": "1982-04-18", "relationship": "Spouse"}
        ]
    },
    {
        "employee_id": "11223",
        "name": "Emily Davis",
        "dob": "1995-09-30",
        "email": "emily.davis@example.com", 
        "hsa_eligible": True,
        "fsa_eligible": False,
        "cobra_status": "Not Applicable",
        "dependents": []
    }
]

#### 2.2. Mocking HSA/FSA Claims Data
Let's create tables in Supabase for the mock data and then we'll use the Supabase client to get the data. The table structure should be as follows:
mock_claims = [
    {
        "claim_id": "C1001",
        "employee_id": "12345",
        "date": "2024-03-01",
        "amount": 150.00,
        "type": "HSA",
        "description": "Doctor's Visit",
        "status": "Pending"
    },
    {
        "claim_id": "C1002",
        "employee_id": "67890",
        "date": "2024-03-05",
        "amount": 75.00,
        "type": "FSA",
        "description": "Prescription",
        "status": "Pending"
    },
    {
        "claim_id": "C1003",
        "employee_id": "13579",
        "date": "2024-03-10",
        "amount": 200.00,
        "type": "HSA",
        "description": "Dental Cleaning",
        "status": "Approved"
    },
    {
        "claim_id": "C1004",
        "employee_id": "24680",
        "date": "2024-03-15",
        "amount": 100.00,
        "type": "FSA",
        "description": "Eyeglasses",
        "status": "Pending"
    }
]

#### 2.3. Mocking Cobra Data
Let's create tables in Supabase for the mock data and then we'll use the Supabase client to get the data. The table structure should be as follows:
mock_cobra_events = [
    {
        "employee_id": "67890",
        "event_type": "Termination",
        "event_date": "2024-02-15",
        "cobra_start_date": "2024-03-01",
        "cobra_end_date": "2025-08-31"
    },
    {
        "employee_id": "12345",
        "event_type": "Termination",
        "event_date": "2024-02-15",
        "cobra_start_date": "2024-03-01",
        "cobra_end_date": "2025-08-31"
    },
    {
        "employee_id": "13579",
        "event_type": "Termination",
        "event_date": "2024-02-15",
        "cobra_start_date": "2024-03-01",
        "cobra_end_date": "2025-08-31"
    },
    {
        "employee_id": "11223",
        "event_type": "Termination",
        "event_date": "2024-02-15",
        "cobra_start_date": "2024-03-01",
        "cobra_end_date": "2025-08-31"
    }
]

#### 2.4. Life Event Data for Dependent Verification
Let's create tables in Supabase for the mock data and then we'll use the Supabase client to get the data. The table structure should be as follows:
mock_life_events = [
    {
        "event_id": "LE1001",
        "employee_id": "12345",
        "event_type": "Birth",
        "event_date": "2024-02-01",
        "dependent": {
            "name": "Carol Smith",
            "dob": "2024-02-01",
            "relationship": "Child",
            "verification_status": "Pending",
            "documents_required": ["Birth Certificate", "Social Security Card"]
        }
    },
    {
        "event_id": "LE1002", 
        "employee_id": "67890",
        "event_type": "Marriage",
        "event_date": "2024-01-15",
        "dependent": {
            "name": "Sarah Johnson",
            "dob": "1994-05-20",
            "relationship": "Spouse",
            "verification_status": "Verified",
            "documents_required": ["Marriage Certificate"]
        }
    },
    {
        "event_id": "LE1003",
        "employee_id": "12345",
        "event_type": "Adoption",
        "event_date": "2024-03-10",
        "dependent": {
            "name": "David Smith",
            "dob": "2020-08-15",
            "relationship": "Child",
            "verification_status": "In Review",
            "documents_required": ["Adoption Papers", "Court Documents"]
        }
    },
    {
        "event_id": "LE1004",
        "employee_id": "13579",
        "event_type": "Birth",
        "event_date": "2024-02-01",
        "dependent": {
            "name": "Emma Williams",
            "dob": "2024-02-01",
            "relationship": "Child",
            "verification_status": "Pending",
            "documents_required": ["Birth Certificate", "Social Security Card"]
        }
    }
]

#### 2.5. Wellness Data from Wellness Apps
Let's create tables in Supabase for the mock data and then we'll use the Supabase client to get the data. The table structure should be as follows:
wellness_data = [
    {
        "employee_id": "12345",
        "consent_status": "Opted In",
        "data_source": "Fitbit",
        "timestamp": "2024-02-15T08:30:00",
        "metrics": {
            "heart_rate": {
                "resting": 72,
                "max": 142,
                "min": 58
            },
            "sleep_data": {
                "hours": 6.5,
                "quality": "Fair",
                "deep_sleep_percentage": 20
            },
            "activity_level": {
                "steps": 8500,
                "active_minutes": 45,
                "calories_burned": 2100
            },
            "stress_indicators": {
                "hrv": 45,  # Heart Rate Variability
                "stress_score": "Medium"
            }
        },
        "risk_factors": ["Sleep Deficiency", "Moderate Stress Levels"]
    },
    {
        "employee_id": "67890",
        "consent_status": "Opted In",
        "data_source": "Apple Watch",
        "timestamp": "2024-02-15T09:00:00",
        "metrics": {
            "heart_rate": {
                "resting": 82,
                "max": 165,
                "min": 65
            },
            "sleep_data": {
                "hours": 7.5,
                "quality": "Good",
                "deep_sleep_percentage": 25
            },
            "activity_level": {
                "steps": 4200,
                "active_minutes": 20,
                "calories_burned": 1800
            },
            "stress_indicators": {
                "hrv": 35,
                "stress_score": "High"
            }
        },
        "risk_factors": ["Elevated Heart Rate", "High Stress", "Low Activity"]
    },
    {
        "employee_id": "13579",
        "consent_status": "Opted In",
        "data_source": "Samsung Health",
        "timestamp": "2024-02-15T08:45:00",
        "metrics": {
            "heart_rate": {
                "resting": 68,
                "max": 135,
                "min": 55
            },
            "sleep_data": {
                "hours": 8.2,
                "quality": "Excellent",
                "deep_sleep_percentage": 30
            },
            "activity_level": {
                "steps": 12000,
                "active_minutes": 75,
                "calories_burned": 2600
            },
            "stress_indicators": {
                "hrv": 65,
                "stress_score": "Low"
            }
        },
        "risk_factors": []
    }
]

#### 2.6. Mocking Chat History
Let's create tables in Supabase for the mock data and then we'll use the Supabase client to get the data. The table structure should be as follows:
mock_chat_history = [
    {
        "employee_id": "12345",
        "chat_history": "..."
    },
    {
        "employee_id": "67890",
        "chat_history": "..."
    },
    {
        "employee_id": "13579",
        "chat_history": "..."
    },
    {
        "employee_id": "11223",
        "chat_history": "..."
    }
]

#### 2.7. Mocking External Systems (Using Functions):

Python:
def mock_hsa_fsa_system(claim_id, action):
    """Simulates interaction with an HSA/FSA system."""
    print(f"Mock HSA/FSA System: Processing {action} for claim {claim_id}")
    # Simulate approval or denial
    if action == "approve":
        return "Approved"
    elif action == "deny":
        return "Denied"
    else:
        return "Error"

def mock_cobra_system(employee_id, action):
    """Simulates interaction with a COBRA administration system."""
    print(f"Mock COBRA System: Processing {action} for employee {employee_id}")
    #Simulate sending notifications
    if action == "send_election_notice":
      return "Sent"
    else:
      return "Error"

def mock_email_system(to_email, subject, message):
    """Simulates sending an email."""
    print(f"Mock Email System: Sending email to {to_email} \nSubject: {subject}\nMessage: {message}\n")
    return "Sent"


#### 2.8. Mocking Policy Documents (RAG Agent Data)
Let's create mock policy documents in Supabase and then we'll use the Supabase client to get the data. The table structure should be as follows:
mock_policy_documents = [
    {
        "policy_id": "P1001",
        "policy_name": "HSA Eligibility Policy",
        "policy_text": "..."
    },
    {
        "policy_id": "P1002",
        "policy_name": "FSA Eligibility Policy",
        "policy_text": "..."
    },
    {
        "policy_id": "P1003",
        "policy_name": "COBRA Eligibility Policy",
        "policy_text": "..."
    }, 
    {
        "policy_id": "P1004",
        "policy_name": "Wellness Policy",
        "policy_text": "..."
    },
    {
        "policy_id": "P1005",
        "policy_name": "Dependent Verification Policy",
        "policy_text": "..."
    }
]   

Example: hsa_policy.txt:
HSA Eligibility Policy

To be eligible for an HSA, an employee must:

1. Be covered under a High Deductible Health Plan (HDHP).
2. Not be enrolled in Medicare.
3. Not be claimed as a dependent on someone else's tax return.

Eligible expenses include:
- Doctor's visits
- Hospital stays
- Prescription drugs
- Dental and vision care

... (more details)

Example: cobra_policy.txt:

COBRA Policy

COBRA (Consolidated Omnibus Budget Reconciliation Act) allows terminated employees to keep their current health insurance.
The employer must provide a 60-day election period notice to former employees.
Former employees who elect COBRA may keep their health coverage for a period of 18 months.

... (more details)

Example: fsa_policy.txt:

FSA Policy

FSA (Flexible Spending Account) allows employees to set aside pre-tax dollars for eligible medical expenses.

... (more details)


#### 2.9. Administration Portal
We'll create an administration portal that allows the admin to manage the employees, claims, life events, wellness data, and cobra events. The admin portal should be a separate page that is accessible by the admin only. It should run on localhost and a different port than the chat window. The admin portal will be developed using Next.js 14, Shadcn UI components, Tailwind CSS, and Lucide Icons.
The admin portal should have the following functionalities:

##### 2.9.1. Data Tab

- View all employees
- View all claims
- View all life events
- View all wellness data
- View all cobra events
- Add new employees
- Add new claims
- Add new life events
- Add new wellness data
- Add new cobra events

- Edit existing employees
- Edit existing claims
- Edit existing life events
- Edit existing wellness data
- Edit existing cobra events

- Delete existing employees
- Delete existing claims
- Delete existing life events
- Delete existing wellness data
- Delete existing cobra events

-Add Policy Documents
-Edit Policy Documents
-Delete Policy Documents

The admin portal will be connected to the Supabase backend and will use the Supabase client to get the data, and send the edited data to the Supabase backend, allow us to add, edit, and delete the data in the Supabase tables through and admin interface.
The admin interface should be a table that displays the data in a table format using the below methods:
- **Component**: PostsTable renders the posts in a table format.
- **Sorting**: Sort by latest date descending.
- **Pagination**: Implement if necessary for more than 100 records.
- **Search**: Search by employee_id, name, email, hsa_eligible, fsa_eligible, cobra_status, dependents, claim_id, date, amount, type, description, status, event_type, event_date, cobra_start_date, cobra_end_date, event_id, event_type, event_date, dependent, verification_status, documents_required, consent_status, data_source, timestamp, metrics, risk_factors, policy_id, policy_name, policy_text.
- **Filter**: Filter by employee_id, name, email, hsa_eligible, fsa_eligible, cobra_status, dependents, claim_id, date, amount, type, description, status, event_type, event_date, cobra_start_date, cobra_end_date, event_id, event_type, event_date, dependent, verification_status, documents_required, consent_status, data_source, timestamp, metrics, risk_factors, policy_id, policy_name, policy_text.
- **Concurrency**:
  - Perform API calls concurrently to improve performance.
- **Caching**:
  - Implement caching strategies for frequently accessed data.
- **Error Handling**:
  - Gracefully handle errors from Supabase.
  - Provide user feedback in the UI for any issues.

  Use Shadcn’s built-in Table components and any official sorting/pagination utilities for uniform styling and functionality.

##### 2.9.2. Intervention Tab
There will be a intervention tab that will display all the intervention tasks in a table format with an action button to approve or deny the task. The intervention tab will be connected to the Supabase table "intervention_tasks" and will use the Supabase client to get the data, and send the edited data to the Supabase backend, allow us to add, edit, and delete the data in the Supabase table through and admin interface. Once the intervention task is complete, the intervention tab will display the results of the task in the chat window.
Additional Implementation for Intervention Flow:
Include timestamps and statuses (e.g., Pending, In Progress, Completed) in the intervention_tasks table.
Enable push notifications or realtime updates (using Supabase’s real-time capabilities) to update the admin portal table automatically.


### 3. CrewAI Agents
The following agents should be created:
- **HSA/FSA Claim Approval Agent**: This agent will be responsible for approving or denying HSA/FSA claims.
- **COBRA Administration Agent**: This agent will be responsible for sending notifications to former employees and handling COBRA elections.
- **Dependent Verification Agent**: This agent will be responsible for verifying the dependents of the employees. It will invoke LLM OCR and vision capabilities to verify the dependents if a new document is uploaded and add the verified dependents to the Supabase table. This agent will also be responsible for sending notifications to the employees when a dependent is verified or when a document is uploaded.
- **Wellness Data Agent**: This agent will be responsible for collecting wellness data from the wellness apps and storing it in the Supabase table.
- **Policy Document Agent**: This agent will be responsible for reading the policy documents and commparing against the user's chat history to answer questions about the policy or run a claim against the policy to validate if the claim is valid or not and inform the user of the results as well as quote from the policy document if the claim in invalid to specify the reason why the claim was denied.
- **Chat History Agent**: This agent will be responsible for storing the chat history in the Supabase table. This agent will be responsible for retrieving the chat history from the Supabase table and displaying it to the user in the chat window if the user requests to see the chat history. This agent can also pass the chat history to other agents to help them with their tasks as directed by the Manager Agent.
- **Life Event Agent**: This agent will be responsible for verifying the life events of the employees. If a new life event document is uploaded, this agent will invoke LLM OCR and vision capabilities to verify the life event and add the life event to the Supabase table. This agent will also be responsible for sending notifications to the employees when a life event is verified or when a document is uploaded. and will be able to pass the life event data to other agents to help them with their tasks as directed by the Manager Agent.
- **Manager Agent**: This agent will be responsible for managing the agents and delegating the tasks to the other agents. It will also be responsible for handling the user requests and delegating the tasks to the other agents.
- **HITL Agent**: This agent will be responsible for handling the Human in the Loop. It will be responsible for reviewing the results of the agents and providing feedback to the agents. It will also be responsible for reviewing the chat history and providing feedback to the agents. If a human intervention is needed, the HITL Agent will be responsible for calling the Manager Agent to delegate the task to the appropriate agent. In this case the HITL Agent will decide why the human intervention is needed and pass its reasoning to the Manager Agent. The manager agent will then decide which agent to delegate the task to to inform the user that a human intervention is needed and ask the user if they would like to proceed with the human intervention. If the user agrees, the anager agent will call the appropraite backend processes to create an intervention task in the admin portal. The intervention task will be a new row in the Supabase table labelled "intervention_tasks" with the task details . The HITL Agent will then be responsible for monitoring the intervention task and providing feedback to the user on the progress of the task. When the task is complete, the HITL Agent will mark the task as complete in the Supabase table and provide feedback to the user on the results of the task in the chat window.

Additional Agent Architecture Notes:

Use asynchronous patterns (e.g., async/await) within each agent for concurrent data fetching or processing.
Design a simple message-passing or queue system for inter-agent communication, ensuring logs are recorded for all handoffs.



## File Structure

The project aims for minimal files without compromising clarity.

r├── README.md
├── app
│   ├── favicon.ico
│   ├── fonts
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
├── backend
├── components
│   └── ui
├── components.json
├── hooks
│   └── use-mobile.tsx
├── instructions
│   └── instructions.md
├── lib
│   └── utils.ts
├── next-env.d.ts
├── next.config.mjs
├── package-lock.json
├── package.json
├── postcss.config.mjs
├── tailwind.config.ts
└── tsconfig.json


**Implementation Note**: 
- All **backend (Python/FastAPI) code**, including **CrewAI** logic, agents, and supporting scripts, should be placed under the `backend` folder.  
- Whenever a Python or CrewAI agent file is created, place it in `backend/`, and ensure any dependencies (e.g., `requirements.txt`) reside there as well.

In summary:

- **`backend/`**  
  - `main.py` or your primary FastAPI entry point  
  - CrewAI agent files (e.g., `manager_agent.py`, `hitl_agent.py`, etc.)  
  - Additional scripts for data handling or API logic  


## Packages and Libraries

### 1. Frontend
- Develop the frontend using Next.js 14, Shadcn UI components, Tailwind CSS, and Lucide Icons.
- Use the Supabase client to get the data from the Supabase tables.
- Use the Supabase client to send the edited data to the Supabase tables.
- axios or fetch for API calls to the FastAPI backend or serverless routes.
- react-query or SWR for data fetching, caching, and real-time updates if desired.


### 2. Backend
- Develop the backend using Python and FastAPI.
- Use the Supabase client to get the data from the Supabase tables.
- Use the Supabase client to send the edited data to the Supabase tables.
- uvicorn for local development server.
- pydantic for data validation.
- pytest for unit tests.

### 3. CrewAI
- Use the CrewAI library to create the agents.
- Use the CrewAI library to run the agents.
- Use the CrewAI library to track the agents and the calls to the agents.
- Use the CrewAI library to track the LLM calls and usage.
- Use the CrewAI library to handle the HITL and Ethical agents.
- Use the CrewAI library to handle the RAG agents.

Implement a manager class/factory that instantiates each agent and orchestrates their tasks.


## Documentation and Code Examples

### 1. Crew AI Documentation
- The Crew AI documentation is available at https://docs.crewai.com

### 2. Supabase Documentation
- The Supabase documentation is available at https://supabase.com/docs

### 3. FastAPI Documentation
- The FastAPI documentation is available at https://fastapi.tiangolo.com/

### 4. Next.js Documentation
- The Next.js documentation is available at https://nextjs.org/docs

### 5. Shadcn UI Documentation
- The Shadcn UI documentation is available at https://ui.shadcn.com/docs

### 6. Tailwind CSS Documentation
- The Tailwind CSS documentation is available at https://tailwindcss.com/docs

### 7. Lucide Icons Documentation
- The Lucide Icons documentation is available at https://lucide.dev/docs


### Authentication and API Keys

- **Gemini API**:
  - Obtain credentials from Gemini API.
  - Ensure secure storage of apiKey.
  - Use the Gemini API to generate the LLM and RAG responses.
- **Supabase**:
  - Obtain credentials from Supabase.
  - Ensure secure storage of the Supabase client and the Supabase URL.
  - Use the Supabase client to get the data from the Supabase tables.
  - Use the Supabase client to send the edited data to the Supabase tables.
- **CrewAI**:
  - Obtain credentials from CrewAI.
  - Ensure secure storage of the CrewAI client and the CrewAI URL.
  - Use the CrewAI client to get the data from the CrewAI tables.
  - Use the CrewAI client to send the edited data to the CrewAI tables.

### Concurrency and Performance

- **Concurrent Processing**:
  - Use Promise.all for concurrent API calls to Gemini, Supabase, and CrewAI.
  - Limit concurrency if necessary to avoid rate limits.
- **Caching**:
  - Implement caching strategies for Gemini, Supabase, and CrewAI.

- **Performance Monitoring**:
  - Use logging and a metrics dashboard Datadog to monitor memory usage, response times, and throughput.
  - Implement circuit breakers or retries when dealing with external service calls.



### Error Handling

- **API Errors**:
  - Gracefully handle errors from Gemini, Supabase, and CrewAI.
  - Provide user feedback in the UI for any issues.
- **Validation**:
  - Validate user inputs.
- **Error Logging**:
  - Create custom error classes (e.g., CrewAIError, SupabaseError) to differentiate error sources.
  - Maintain an error log in the Supabase table called "error_log" for debugging and auditing.


### Styling and UI Components

- **Tailwind CSS**:
  - Utilize Tailwind classes for styling components.
  - Ensure responsiveness across different screen sizes.
- **Shadcn UI Components**:
  - Leverage pre-built components where applicable.
- **Icons**:
  - Use Lucide Icons for consistent iconography.

- **Design System**:
  - Follow a consistent design system (e.g., color palette, typography).


### State Management
- **Server State**:
  - Consider React Query (or SWR) to manage server-state, caching, and re-fetching logic seamlessly.
- **Local State**:
  - Use React's useState and useEffect hooks for local component state.
- **Global State**:
  - If needed, use Context API or state management libraries for global state (e.g., list of subreddits).

### Accessibility and SEO

- **Accessibility**:
  - Ensure all interactive elements are accessible via keyboard.
  - Use semantic HTML elements.
- **SEO**:
  - Optimize pages for search engines with proper meta tags.

### Security Considerations

- **API Keys**:
  - Do not expose API keys in the client-side code.
  - Use environment variables and server-side fetching where possible.
- **Input Sanitization**:
  - Sanitize user inputs to prevent XSS attacks.

  ### Testing

- **Unit Tests**:
  - Write unit tests for utility functions in lib/utils.ts.
- **Integration Tests**:
  - Test API interactions with mock data.
- **End-to-End Tests**:
  - Use testing frameworks like Cypress for end-to-end testing.

  ### LLM Usage Monitoring
  - I'll provide you with the LangSmith API key. You need to ensure it's stored in the .env file securely and not exposed in the codebase.
  - Use LangSmith to track the LLM calls and usage.
  - User LangSmith to track the RAG calls and usage.

