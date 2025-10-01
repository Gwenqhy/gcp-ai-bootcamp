# Building Low Code Agents with Google Cloud's Conversational Agents

## Overview
In this lab, you'll deploy a pre-built low code generative AI agents using Google Cloud's Conversational Agents tool. We'll cover the essential concepts and walk you through the initial steps to get your first agent up and running.

With Conversational Agents, AI Agents can be created in just a few steps. For today's lab, we will be importing a Pre-Built Agent so that you can see how it works and play around with it
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

In line 8, replace the server URL with an **active webhook**
- Paste the link in a new tab and follow the instructions to the prebuilt tool installer to deploy webhooks
```bash
https://cloud.google.com/dialogflow/cx/docs/concept/playbook/prebuilt/travel#tool-setup
```
![Tool installer](https://github.com/user-attachments/assets/e7e3dc66-0b86-432a-8c5a-3c5af6ce389c)

- Ensure that the user running the `installer.py` script has the following minimum
permissions in IAM & Admin > your-project-number@developer.gserviceaccount.com > (Pencil icon) Edit Principal
```
https://console.cloud.google.com/
```
    Service Usage Admin
    Cloud Functions Developer
    Firebase
    Develop Admin
    Storage Object User
  
- Upload the unzipped Installer folder into your GCP Cloud Shell Editor
![Pre-built Agent Settings](https://github.com/user-attachments/assets/41e99e79-c297-4c83-bfc1-b7a91f5deb75)


- Run in terminal
```bash
cd <Installer_folder_path>
```
- Create and Activate Virtual Environment (Linux):

   | Environment | Command to Create Venv | Command to Activate Venv | 
   | :--- | :--- | :--- | 
   | Linux/macOS (Bash) | `python3 -m venv venv` | `source venv/bin/activate` | 
   | Windows (Command Prompt) | `python -m venv venv` | `venv\Scripts\activate` | 
   | Windows (PowerShell) | `python -m venv venv` | `venv\Scripts\Activate.ps1` |


- Install Dependencies:
```
pip install -r requirements.txt
```

- Authenticate the gcloud CLI: This command ensures your local terminal has the necessary permissions to communicate with your Google Cloud Project. Follow the browser prompts to sign in.
```
gcloud auth application-default login
```

- To deploy webhooks for our prebuilt travel agent
```
python installer.py --project-id=<YOUR_PROJECT_ID> --prebuilt-id=travel
```

- Paste the webhooks into its respective tool's schema, under server URL 

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






