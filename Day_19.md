# Day-19: Generative-AI: Project 1 (Chat Completion)

## Table of Contents
- [Chat Completion Models vs. Text Generation Models](#chat-completion-models-vs.-text-generation-models)
- [Kay Conceps](#key-concepts)
- [Steps to Make Azure ML](#steps-to-make-azure-ml)
- [URI vs. URL](#uri-vs-url)
- [Code Explanation](#code-explanation)
- [Key Vault](#key-vault)
- [Container Registry](#container-registry)
- [Private with Internet Outbound](#private-with-internet-outbound)
- [Private with Approved Outbound](#private-with-approved-outbound)

---
## Generative-AI: Project 1 (Chat Completion)

### Chat Completion Models vs. Text Generation Models**

**Chat Completion Models**
- **Purpose**: Interactive conversation and dialogue, simulating human-like interactions.
- **Applications**: Chatbots, virtual assistants, customer support.
- **Examples**: ChatGPT, DialoGPT, Azure Bot Service.

**Text Generation Models**
- **Purpose**: Create coherent and contextually relevant text based on a given prompt.
- **Applications**: Content creation, creative writing, automated reporting.
- **Examples**: GPT-3, GPT-4, T5, BERT (when fine-tuned for generation tasks).
- **Example Use Case**: Generating a story from a prompt like "Once upon a time...".

---

### Key Concepts
#### Azure ML
**Azure Machine Learning (Azure ML)** is a cloud service for building, training, and deploying machine learning models.

#### Workspace
An **Azure ML Workspace** is a central hub for managing all resources, experiments, and projects related to machine learning.

#### Model Catalog
The **Model Catalog** is a repository within Azure ML for storing, managing, and versioning machine learning models.

#### Serverless API
- **Model Interaction**: Charges based on token usage, ideal for usage-based cost efficiency.

#### Managed Compute Services
- **Model Deployment**: Requires deploying a VM, incurs costs even when idle.

#### API (Application Programming Interface)
- A set of functions and procedures allowing the creation of applications that access the features or data of an operating system, application, or other service.

#### Virtual Environment
- **Virtual environments** help create isolated Python environments for your projects, ensuring dependencies are managed and the system remains clean.

---

### Steps to Make Azure ML

**1. Create a Resource Group (RG)**
- Open the Azure portal.
- Select "Resource groups" and click "Add".
- Enter Subscription, Resource group name, and Region.
- Click "Review + create" and then "Create".

**2. Create an Azure Machine Learning Workspace**
- Go to Azure Machine Learning in the Azure portal and select "Create".
- Fill in the necessary details:
  - Subscription
  - Resource group (use the one created in step 1)
  - Workspace name
  - Region
- Choose **Public** under Networking for testing.
- Click "Review + create" and then "Create".

**3. Launch Studio and Select Model**
- Once the workspace is ready, open the Azure Machine Learning Studio.
- Navigate to the **Model catalog**.
- Select a model, such as **Meta-Llama-3-70B-Instruct**.
- Provide the deployment name.

**4. Install Azure CLI**
- Follow [official instructions](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) to install Azure CLI.

**5. Log in to Azure CLI**
- Open the terminal and execute:
  ```sh
  az login
  ```
- Log in via the web browser window that opens.

**6. Write the Code for the Chatbot**
- Example Python code:
  ```python
  import openai

  openai.api_key = "your_openai_api_key"

  def chatbot_response(prompt):
      response = openai.Completion.create(
          engine="davinci",
          prompt=prompt,
          max_tokens=150
      )
      return response.choices[0].text.strip()

  user_input = input("You: ")
  print("Bot:", chatbot_response(user_input))
  ```

**7. Deploy the Chatbot**

#### Create a VM
- In the Azure portal, create a VM.
- Allow access to all ports.
- Choose appropriate size and OS (e.g., Windows Server or Ubuntu).

#### Connect using RDP
- Use an RDP client to connect to the VM using its public IP and credentials.

**8. Configure the VM as a Web Server**
- Install necessary web server software (e.g., IIS for Windows or Apache/Nginx for Linux).

**9. Install Python and Necessary Packages**
- Ensure Python is installed on the VM.

**10. Upload and Run Your Code**
- Upload your code to the VM, typically in the `inetpub/wwwroot` folder.
- Open the command prompt or terminal, navigate to your project directory, and run:
  ```sh
  pip install -r requirements.txt
  ```
  Ensure `requirements.txt` contains all dependencies.
- Navigate to the chat folder and run:
  ```sh
  streamlit run chat_fe.py --server.port 8080
  ```

**11. Configure Inbound Rule for Port 8080**
- In the Azure portal, go to your VM's network security group (NSG).
- Add an inbound security rule for port 8080.

---

### URI vs. URL

- **URI (Uniform Resource Identifier)**: Identifies a resource on the internet or within a system. URI can include various components like a scheme (e.g., http), a path (e.g., /page), and a query (e.g., ?id=123).
- **URL (Uniform Resource Locator)**: A type of URI specifying the address and protocol to access a resource. A URL (Uniform Resource Locator) is a type of URI that specifies the address of a resource, including the protocol (e.g., http, https, ftp) and location (e.g., www.example.com/page).

---

### Code Explanation**

#### os Package
- **Purpose:** Interact with the operating system.
- **Functions:** File and directory manipulation, process management, environment variable access.

#### dotenv Package
- **Purpose:** Load environment variables from a `.env` file.
- **load_env:** 
  - **Purpose:** Load variables from `.env` to environment.
  - **Usage:** Read configuration settings from a `.env` file.
- **find_env:**
  - **Purpose:** Locate the `.env` file.
  - **Usage:** Automatically find the `.env` file for use with `load_dotenv`.

#### azure.ai.ml
- **Purpose:** Authentication for Azure Machine Learning.
- **MLClient:** Authenticates with Azure ML.
- **DefaultAzureCredential:**
  - **Purpose:** Authenticate applications/services.
  - **Fetches:** tenantID, subscriptionID.

#### langchain_community.chat_models.azureml_endpoint
- **Purpose:** Integrate with Azure ML endpoints for chat models.
- **AzureMLChatOnlineEndpoint:** Interact with chat models on Azure ML.
- **CustomOpenAIChatContentFormatter:** Format chat messages for custom input/output.
- **AzureMLEndpointApiType:** Define API type for Azure ML endpoint.

#### langchain_core.messages
- **Purpose:** Manage message structures for conversations.
- **HumanMessage:** Message from a human user.
- **AIMessage:** Message from AI/language model.

#### os.getenv
- **Purpose:** Retrieve value of an environment variable.
- **Usage:** Access configuration settings, credentials, or other data.

#### Virtual Environment (venv)
- **Purpose:** Create isolated Python environments.
- **Commands:**
  - `python -m venv myazure`: Create virtual environment.
  - `source myazure/bin/activate`: Activate virtual environment.

#### Conversation Memory
- **Purpose:** Manage conversation history and context.
- **ConversationBufferMemory:**
  - **Purpose:** Store conversation history.
  - **Usage:** Maintain context over multiple interactions.
- **ConversationChain:**
  - **Purpose:** Use memory for context-aware conversations.
  - **Usage:** Integrate language model with memory for multi-turn interactions.

#### max_token_limit
- **Purpose:** Specify maximum number of tokens in input/output.
- **Usage:** Manage model's computational load, ensure manageable response sizes.

#### Streamlit State Management
- **st.session_state:**
  - **Purpose:** Persist data across user interactions.
  - **Usage:** Maintain user inputs, store computations, manage workflows.

#### Streamlit Chat Components
1. **st.chat_input("Chat with myllama :robot_face:")**
   - **Purpose:** Create chat input box.
   - **Usage:** Capture user's input text.
2. **if input_text:**
   - **Purpose:** Check if user entered text.
   - **Usage:** Execute following code only if input exists.
3. **with st.chat_message('user'):**
   - **Purpose:** Display user's message.
   - **Usage:** Show user's input in chat interface.
4. **Displaying Messages in Streamlit:**
   - **st.chat_message**: Creates chat message bubbles.
   - **st.markdown**: Formats and displays message content.
5. **st.session_state.chat_history.append({'role':'user','text':input_text})**
   - **Purpose:** Update chat history with user's message.
   - **Usage:** Track conversation history.
6. **chat_response = bot.chatbot_conversation(input_txt=input_text, memory=st.session_state.memory)**
   - **Purpose:** Generate chatbot response.
   - **Usage:** Get reply from chatbot based on user's message.
7. **with st.chat_message('assistant'):**
   - **Purpose:** Display chatbot's response.
   - **Usage:** Show chatbot's reply in chat interface.
8. **st.session_state.chat_history.append({'role':'assistant', 'text':chat_response})**
   - **Purpose:** Update chat history with chatbot's response.
   - **Usage:** Maintain complete conversation log.

**Example Code: Streamlit and Chatbot**

1. **Create Virtual Environment**:
   ```sh
   python -m venv myazure
   source myazure/bin/activate
   ```

2. **Navigate to Directory and Install Packages**:
   ```sh
   cd specific_directory
   pip install -r requirements.txt
   ```

3. **Run the Streamlit Application**:
   ```sh
   streamlit run app.py --server.port 8080
   ```

**Running the Code**
1. **Input Text**:
   ```python
   input_text = st.chat_input("Chat with myllama :robot_face:")
   ```

2. **User Input Check**:
   ```python
   if input_text:
   ```

3. **Display User Message**:
   ```python
   with st.chat_message('user'):
       st.markdown(input_text)
   ```
4. **.env File Configuration**:
   - Ensure your `.env` file contains the necessary environment variables.
   - Example:
     ```env
     OPENAI_API_KEY=your_openai_api_key
     ```

5. **Update Chat History**:
   ```python
   st.session_state.chat_history.append({'role':'user', 'text':input_text})
   ```

6. **Generate Chatbot Response**:
   ```python
   chat_response = bot.chatbot_conversation(input_txt=input_text, memory=st.session_state.memory)
   ```

7. **Display Chatbot Response**:
   ```python
   with st.chat_message('assistant'):
       st.markdown(chat_response)
   ```

8. **Update Chat History**:
   ```python
   st.session_state.chat_history.append({'role':'assistant', 'text':chat_response})
   ```

**Launching Applications on Servers**
- Containerize the application with Docker before launching.
- Create a Docker image and deploy the application.

---

### Key Vault
- **Purpose**: Key Vault is used to generate and manage cryptographic keys, including symmetric keys used for data encryption.
- **Functionality**: It provides a secure environment for key management, ensuring that keys are protected from unauthorized access and are available for use in your applications.
- **Usage**: You can use Key Vault to securely store and access secrets, keys, and certificates, ensuring your application data remains protected.

---

### Container Registry
- **Purpose**: Azure Container Registry is used to store and manage container images for Docker containers.
- **Functionality**: It provides a central repository for container images, allowing you to deploy and manage your containerized applications seamlessly.
- **Usage**: When you need to deploy models or host Docker containers, Container Registry comes into play, ensuring your images are securely stored and easily accessible for deployment.

---
### Private with Internet Outbound
- **Description**: This configuration ensures that your application cannot be accessed by anyone, but you still have internet access to install packages or update software.
- **Functionality**: Outbound data movement is unrestricted, allowing your application to access the internet for necessary updates while remaining isolated from incoming traffic.
- **Usage**: This is preferable in industries where security is paramount, but internet access is required for updates or installations.

---
### Private with Approved Outbound
- **Description**: This configuration restricts outbound data movement to approved targets only.
- **Functionality**: It ensures that your application can only send data to specific, pre-approved endpoints, enhancing security by controlling data flow.
- **Usage**: This is ideal for environments where strict data control is necessary, ensuring that data can only be sent to trusted destinations.

---
