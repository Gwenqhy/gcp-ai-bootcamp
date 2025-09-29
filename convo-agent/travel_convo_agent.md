# Building Low Code Agents with Google Cloud's Conversational Agents

## Overview
In this lab, you'll deploy a pre-built low code generative AI agents using Google Cloud's Conversational Agents tool. We'll cover the essential concepts and walk you through the initial steps to get your first agent up and running.

This section focuses on deploying the Google Maps Places API functionality to Cloud Run and using the resulting server URL to power the Travel Pre-Built Agent's dynamic search capabilities. This deployment is required to enable the Conversational Agent to dynamically search for points of interest using its places_search tool.


## 🚀 Part 1: Step-by-Step Deployment: Places Search Webhook
This guide details the steps necessary to deploy the places-search-webhook service to Google Cloud Run and integrate it as an OpenAPI tool in Conversational Agent.

### 1. Google Cloud Prerequisites
First, ensure your Google Cloud Project is set up correctly and has the necessary APIs enabled.

#### 1.1. Enable Required APIs
Navigate to the Google Cloud Console API Library and enable the following services for your project:

- Dialogflow API

- Cloud Run API

- Cloud Build API

- Google Maps Platform (Specifically, the Places (New) API for search functionality).

#### 1.2. Secure Your Google Maps API Key
Go to APIs & Services > Credentials.

Click + Create Credentials and select API Key.

Crucially, restrict this key for security:

  Click into the newly created API key.

  Under API restrictions, select Restrict Key and add the Places API from the dropdown list.

Copy the generated key. This will be used in the deployment command.

### 2. Local Development Setup
These steps prepare your local environment for building the container image.
```
git clone https:// 
cd  
```

#### 2.1 Create and Activate Virtual Environment (Linux):

   | Environment | Command to Create Venv | Command to Activate Venv | 
   | :--- | :--- | :--- | 
   | Linux/macOS (Bash) | `python3 -m venv venv` | `source venv/bin/activate` | 
   | Windows (Command Prompt) | `python -m venv venv` | `venv\Scripts\activate` | 
   | Windows (PowerShell) | `python -m venv venv` | `venv\Scripts\Activate.ps1` |


#### 2.2 Install Dependencies:
```
pip install -r requirements.txt
```

#### 2.3 Authenticate the gcloud CLI:
This command ensures your local terminal has the necessary permissions to communicate with your Google Cloud Project. Follow the browser prompts to sign in.
```
gcloud auth application-default login
```

### 3. Build and Deploy to Cloud Run
This phase transforms your Python code into a runnable container and deploys it as a serverless service.

#### 3.1. Build and Push the Container Image
This command reads your Dockerfile, builds the image, and pushes it to the Google Container Registry (gcr.io).

```gcloud builds submit --tag gcr.io/<PROJECT_ID>/travel-search-webhook .```

(Replace <PROJECT_ID> with your actual Google Cloud Project ID.)

#### 3.2. Deploy the Service to Cloud Run
This command deploys the image, configures the necessary public access (--allow-unauthenticated), and securely injects your API Key as an environment variable (MAPS_API_KEY).

⚠️ IMPORTANT: Replace the placeholder with your actual Project ID and Maps API Key.
```
gcloud run deploy travel-search-webhook \
  --image gcr.io/<PROJECT_ID>/travel-search-webhook \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars "MAPS_API_KEY=YOUR_ACTUAL_API_KEY_GOES_HERE"
```

#### 3.3. Final Conversational Agents Endpoint
Upon successful deployment, Cloud Run will provide a **Service URL**. This will go to your places_search_tool later.

![Project Screenshot](https://github.com/user-attachments/assets/ea3e390f-65bb-4940-a5cb-4d9f69feac33)


## 🚀 Part 2: Building a Google Cloud's Conversational Agents


### Step 1: Go to Conversational Agents page
- Open a new tab and copy the URL below (Replace ```<PROJECT_ID>``` with your Project ID):

```
https://conversational-agents.cloud.google.com/projects/<PROJECT_ID>/prebuilt
```

- In the next page (shown below), select the **Travel** agent (You might have to scroll all the way down).
> [!NOTE]  
> You might see multiple travel-themed agents. Make sure to select the agent that is named **Travel**.

![Select Travel page](./images/select_travel.png)

- Click on **Import Agent**
![Import Agent page](./images/import_agent.png)

- For the settings, give your Agent a name **(e.g. John Travel)**
- Leave everything else as default and click on **Create**
![Pre-built Agent Settings](./images/prebuilt_agent_settings.png)

- Next, in the left panel click on **Tools**
- Click into **places_search** tool.
![Tools Page](./images/tools_page.png)

- Scroll down to **Schema**
- Under Server URL, replace the sample URL with your active URL deployed from Part 1
```bash
https://travel-places-search-288715243473.us-central1.run.app
```
- Once done, click **Save**
![Replace URL](./images/replace_server_url.png)

- Repeat this for the other tools - remember to always click **Save** after you edit!
  
| Tool Name         | URL |
| :---------------- | :------ |
| places_search     | https://travel-places-search-288715243473.us-central1.run.app    |
| hotel_booking     | https://travel-book-hotel-288715243473.us-central1.run.app       |
| hotel_search      | https://travel-places-search-288715243473.us-central1.run.app    |
| get_user_profile  | https://travel-get-user-profile-288715243473.us-central1.run.app |

- Once you're done, you are ready to test your travel agent! 
- Head back to Playbooks and select **Travel Steering**
- Click on the **Chat** icon at the top to toggle open the simulator
![Toggle open chat](./images/toggle_chat.png)

### Ask away!
You can ask for recommendations, more information about certain places and also try to get it to book a hotel for you (of course it'll be a simulated booking, not an actual one!)
- Here are some as starters:
    - Beach vacation in Phuket
    - Hotels near the beach in Phuket
    - Hotels in Sentosa

For example: When asking about certain places you should be able to see that the ```places_search``` tool gets triggered to do a Google Maps API call and retrieve places related to your query. And that's how you get your agent to interact with other systems to enrich it.

![Tools Trigger](./images/trigger_tool.png)






