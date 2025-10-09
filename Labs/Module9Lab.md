# Guided Lab: Securing Applications by Using Amazon Cognito

## Lab Overview and Objectives

Building web applications with secure user authentication and authorization can be challenging. **Amazon Cognito** simplifies this by enabling developers to add sign-up, sign-in, and advanced security features easily.

In this lab, you will:

- Configure an **Amazon Cognito user pool** to manage users and their access to a web application.
- Create an **Amazon Cognito identity pool** to authorize users when the app calls **Amazon DynamoDB**.

### After completing this lab, you should be able to:

- Create an Amazon Cognito user pool.
- Add users to the user pool.
- Update an example application to use the user pool for authentication.
- Configure an Amazon Cognito identity pool.
- Update the application to use the identity pool for authorization.

---

## Duration
**Estimated time:** ~60 minutes

---

## AWS Service Restrictions
In this lab environment, access to AWS services is **restricted** to those required for the lab. Attempting to use other services may result in errors.

---

## Scenario

You have the **Birds web application**, built with a **Node.js** server running on **AWS Cloud9** and an **Amazon S3 bucket** with static website hosting.

The application tracks students’ bird sightings with the following components:

- A **home page**
- An **educational page**
- Three **protected pages**, accessible only after authentication:
  - Sightings page — view past sightings
  - Reporting page — report new sightings
  - Administrator page — for admin operations

Your task is to **add authentication and authorization** to the application for these protected pages.

---

## Architecture Stages

### Starting Architecture
Initial components used to install and run the application.

### Intermediate Architecture
Integration of **Amazon Cognito user pool** for authentication.

#### Steps
| Step | Explanation |
|------|--------------|
| 1 | User requests access to a protected page. |
| 2 | Request routes to Node.js application server. |
| 3 | Application redirects to Amazon Cognito managed UI. |
| 4a | User is authenticated, and an access token is returned. |
| 4b | The token is stored in the browser’s local storage (expires in 3,600 seconds). |
| 5 | Application validates the token and returns the requested page. |
| 6 | Page is delivered via **Amazon CloudFront**. |

### Final Architecture
Integration of **Amazon Cognito identity pool** for authorization and admin access.

#### Steps
| Step | Explanation |
|------|--------------|
| 1–6 | Same as intermediate architecture. |
| 7 | User initiates a query to DynamoDB. |
| 8 | Application sends token to Cognito identity pool for temporary AWS credentials. |
| 9 | Application queries DynamoDB and returns data to the page via CloudFront. |

---

## Accessing the AWS Management Console

1. Choose **Start Lab** at the top of the instructions.
2. Wait for the environment to load (green circle icon).
3. Choose the **AWS** link in the top-left corner to open the console.
4. If pop-ups are blocked, allow them in your browser.
5. Arrange the console tab beside these instructions for easy navigation.

---

## Task 1: Preparing the Lab Environment

### Steps

1. Open **AWS Cloud9 IDE** using the `Cloud9url` from **AWS Details**.
2. In a text editor, prepare a file to store the following:

```

S3 bucket:
CloudFront distribution domain:
User pool ID:
App client ID:
Amazon Cognito domain prefix:
Identity pool ID:

````

3. In the **AWS Cloud9 terminal**, run:

```bash
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACACAD-3-113230/10-lab-mod9-guided-Cognito/code.zip
unzip code.zip
cd resources
. ./setup.sh
````

4. Record the output values (S3 bucket and CloudFront domain).

5. Edit `website/scripts/config.js` and replace `<cloudfront-domain>`:

   ```js
   CONFIG.BASE_NODE_SERVER_STR = "https://d123456acbdef.cloudfront.net";
   ```

6. Upload website files to S3:

   ```bash
   cd /home/ec2-user/environment
   aws s3 cp website s3://<s3-bucket>/ --recursive --cache-control "max-age=0"
   ```

7. Start the Node.js server:

   ```bash
   cd /home/ec2-user/environment/node_server
   npm start
   ```

8. Wait for **CloudFront** status to show **Enabled** before continuing.

---

## Task 2: Reviewing the Birds Website

1. Visit your **CloudFront distribution domain** in a new browser tab.
2. Explore the **home** and **birds** pages.
3. Attempt to access **SIGHTINGS** — login will fail since Cognito isn’t set up yet.
4. Close the app tab but keep **AWS Cloud9** running.

---

## Task 3: Configuring the Amazon Cognito User Pool

### Task 3.1: Creating a User Pool

1. Open **Amazon Cognito** → **User Pools** → **Create user pool**.
2. Configure:

   * Application type: *Traditional web application*
   * Name: `bird_app_client`
   * Username: `email`
   * Return URL:
     `https://<cloudfront-domain>/callback.html`
3. Choose **Create**.
4. Rename the pool to `bird_app` and record the **User Pool ID**.
5. Configure:

   * OAuth 2.0 grant types: *Authorization code grant*, *Implicit grant*
   * Scopes: *Email*, *OpenID*
6. Record **Client ID** and **Cognito domain prefix**.

### Task 3.2: Adding Users

1. Create users:

   * `testuser` → Password: `Lab-password1$`
   * `admin` → Password: `Admin123$`
2. Create a **Group** named `Administrators`.
3. Add `admin` to the `Administrators` group.

---

## Task 4: Updating the Application to Use the User Pool

1. Stop the Node.js server (`Ctrl + C`).

2. In `website/scripts/config.js`, uncomment and update:

   ```js
   CONFIG.COGNITO_DOMAIN_STR = "<cognito-domain>";
   CONFIG.COGNITO_USER_POOL_ID_STR = "<cognito-user-pool-id>";
   CONFIG.COGNITO_USER_POOL_CLIENT_ID_STR = "<cognito-app-client-id>";
   CONFIG.CLOUDFRONT_DISTRO_STR = "<cloudfront-distribution>";
   ```

3. Replace placeholders with your saved values.

4. Re-upload the website:

   ```bash
   cd /home/ec2-user/environment
   aws s3 cp website s3://<s3-bucket>/ --recursive --cache-control "max-age=0"
   ```

5. Update the node server configuration:

   ```bash
   cd /home/ec2-user/environment/node_server
   cp package2.json package.json
   cp libs/mw2.js libs/mw.js
   ```

6. Edit `package.json` and update the start script:

   ```json
   "start": "REGION_STR=us-east-1 USER_POOL_ID_STR=us-east-1_AAAA1111 node index.js"
   ```

---



# Task 5: Testing the User Pool Integration with the Application

In this task, you will test the updated Birds web application to confirm that it now requires authentication via Amazon Cognito.

---

## Steps

1. **Restart the Node.js server** to load the new configuration:

   ```bash
   npm start


2. **Return to the Birds web application** in your browser and refresh the page.

3. Choose **SIGHTINGS**.

   * The list of bird sightings does **not** display because the app now requires authentication.

   > 💡 **Note:** If you encounter a JWT error, open the application in a new browser tab or window.

4. Choose **LOGIN**.

5. On the login prompt, enter the credentials for the **testuser** account.

   * You may be prompted to change the password on first login.

6. After successful login, navigate to **SIGHTINGS** again.

   * The list of bird sightings is now displayed.

   > ✅ This confirms that only authenticated users from the **Amazon Cognito user pool** can access protected pages.

7. Choose **SITEADMIN**.

   * You will see a message: *You need Admin credentials to see Admin page.*

   * Choose **DISMISS**.

8. On the SITEADMIN page, choose **ADMIN LOGIN**.

9. On the login prompt, choose **Sign in as a different user?**

10. Log in using the **admin** credentials.

    * You may be prompted to change the password.

11. After successful login, go to **SITEADMIN** again.

    * The message *Admin page under construction* is displayed.

> 🧩 This test confirms that Amazon Cognito can enforce **role-based access control**, providing secure access to pages based on user roles (e.g., admin vs. regular user).

---

# Task 6: Configuring the Identity Pool

The **Amazon Cognito identity pool** (used for authorization) was pre-created in your lab environment. You will now link it with the user pool.

---

## Steps

1. In the **Amazon Cognito console**, select **Identity pools** from the navigation pane.

2. Choose the **bird_app_id_pool** link.

3. Copy the **Identity pool ID** and save it in your text editor for later use.

4. From the lower pane, choose the **User access** tab.

5. Choose **Add identity provider** → **Amazon Cognito user pool**.

6. Configure the following:

   * **User pool ID:** Select the `bird_app` pool.
   * **App client ID:** Select `bird_app_client`.

7. In **Role settings**, note that a **Default authenticated role** is displayed.

8. Choose **Save changes**.

9. Review the **Authenticated access** section:

   * The authenticated role is configured to assign the default IAM role to logged-in users.

> 🧠 You could create rules for assigning different IAM roles, but for this phase, you’ll use a single default role.

---

# Task 7: Updating the Application to Use the Identity Pool for Authorization

Now that the identity pool is configured, update the Birds web application to use it for DynamoDB access authorization.

---

## Steps

### Step 1: Stop the Node.js Server

In the **AWS Cloud9** terminal, stop the running server:

```bash
Ctrl + C
```

---

### Step 2: Update `config.js`

1. In the **Explorer**, navigate to `website/scripts/`.
2. Open **config.js**.
3. Uncomment the last line (remove `//`) and replace `<cognito-identity-pool-id>` with your **Identity Pool ID**.
4. Save the file (`File → Save`).

---

### Step 3: Update `auth.js`

1. From the same folder (`website/scripts/`), open **auth.js**.

2. Locate the line (around line 91) that defines the AWS credentials:

   ```js
   AWS.config.credentials = new AWS.CognitoIdentityCredentials({
       IdentityPoolId : CONFIG.COGNITO_IDENTITY_POOL_ID_STR,
       Logins : {
           "cognito-idp.us-east-1.amazonaws.com/<cognito-user-pool-id>": token_str_or_null
       }
   });
   ```

3. Replace `<cognito-user-pool-id>` with your actual **User Pool ID**.
   ⚠️ *Do not use the identity pool ID here.*

4. The updated section should look like:

   ```js
   AWS.config.credentials = new AWS.CognitoIdentityCredentials({
       IdentityPoolId : CONFIG.COGNITO_IDENTITY_POOL_ID_STR,
       Logins : {
           "cognito-idp.us-east-1.amazonaws.com/us-east-1_AAAA1111": token_str_or_null
       }
   });
   ```

> This code retrieves temporary AWS credentials from the identity pool using the authentication token from the user pool.

5. Save the file (`File → Save`).

---

### Step 4: Push Updates to S3

Re-upload the updated website to the S3 bucket:

```bash
cd /home/ec2-user/environment
aws s3 cp website s3://<s3-bucket>/ --recursive --cache-control "max-age=0"
```

---

### Step 5: Restart the Node.js Server

If the server is not running, restart it:

```bash
cd /home/ec2-user/environment/node_server
npm start
```

---

# Task 8: Testing the Identity Pool Integration with the Application

In this task, you will test the updated application to confirm it can use the identity pool to obtain **temporary AWS credentials** and interact with **Amazon DynamoDB**.

---

## Steps

1. Return to the browser tab with the Birds application.

2. Choose **HOME**, and refresh the page to load the new code.

3. Choose **REPORT**.

4. Choose **LOGIN**, and sign in using the **admin** credentials.
   *(If you’re already logged in as admin, skip this step.)*

5. After logging in, choose **REPORT** again if needed.

6. Choose **VALIDATE MY TEMPORARY AWS CREDENTIALS**.

   * The app now uses the **identity pool** to:

     * Generate temporary AWS credentials
     * Access the **BirdSightings DynamoDB** table

   * The app confirms successful validation with a message similar to:

     ```
     Your temporary AWS credentials have been configured.
     Connecting to DynamoDB Table..BirdSightings
     Your DynamoDB Table has 0 rows.
     ```

> 🧩 This verifies that authenticated users can securely obtain temporary AWS credentials and access DynamoDB through Cognito.

7. (Optional) In the **DynamoDB console**, verify that the **BirdSightings** table has **0 rows**.

---

# ✅ Conclusion

Congratulations! You have successfully completed all tasks and demonstrated secure user authentication and authorization using Amazon Cognito.

You have accomplished the following:

* Created an **Amazon Cognito user pool**
* Added users and roles to the user pool
* Integrated the **user pool** with your web application for authentication
* Configured an **Amazon Cognito identity pool**
* Integrated the **identity pool** with your application for authorization
* Verified secure, role-based access to DynamoDB using temporary credentials


