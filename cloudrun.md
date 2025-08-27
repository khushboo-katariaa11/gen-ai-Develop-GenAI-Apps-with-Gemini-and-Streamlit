
# **Task 1: Build and Deploy the Application to Cloud Run**

## **Objective**

Deploy a Streamlit application integrated with the Gemini API in Vertex AI to **Google Cloud Run** using **Cloud Shell**, **Docker**, and **Artifact Registry**.

---

## **1. Clone the Repository**

### Open Cloud Shell

* Click on the **Cloud Shell** icon in the top-right corner of the Google Cloud Console.

### Run the following commands:

```bash
git clone https://github.com/GoogleCloudPlatform/generative-ai.git --depth=1
cd generative-ai/gemini/sample-apps/gemini-streamlit-cloudrun
```

---

## **2. Configure the Python Environment**

### Create and activate a virtual environment:

```bash
python3 -m venv gemini-streamlit
source gemini-streamlit/bin/activate
```

### Install dependencies:

```bash
pip install -r requirements.txt
```

---

## **3. Set Required Environment Variables**

The application requires two environment variables for Vertex AI initialization:

* `GOOGLE_CLOUD_PROJECT`: Your **Google Cloud Project ID**
* `GOOGLE_CLOUD_REGION`: Your **deployment region**

### Set variables:

```bash
GOOGLE_CLOUD_PROJECT='[YOUR_PROJECT_ID]'
GOOGLE_CLOUD_REGION='[YOUR_REGION]'
```

Replace `[YOUR_PROJECT_ID]` and `[YOUR_REGION]` with your actual project ID and region.

---

## **4. Build and Push Docker Image to Artifact Registry**

### Set additional environment variables:

```bash
AR_REPO='gemini-repo'
SERVICE_NAME='gemini-streamlit-app'
```

### Create Artifact Registry repository:

```bash
gcloud artifacts repositories create "$AR_REPO" \
  --location="$GOOGLE_CLOUD_REGION" \
  --repository-format=Docker
```

### Build and submit Docker image:

```bash
gcloud builds submit \
  --tag "$GOOGLE_CLOUD_REGION-docker.pkg.dev/$GOOGLE_CLOUD_PROJECT/$AR_REPO/$SERVICE_NAME"
```

> ⏳ *Note: This step may take a few minutes to complete.*

---

## **5. Deploy to Cloud Run**

Deploy the application using the Docker image from Artifact Registry:

```bash
gcloud run deploy "$SERVICE_NAME" \
  --port=8080 \
  --image="$GOOGLE_CLOUD_REGION-docker.pkg.dev/$GOOGLE_CLOUD_PROJECT/$AR_REPO/$SERVICE_NAME" \
  --allow-unauthenticated \
  --region=$GOOGLE_CLOUD_REGION \
  --platform=managed  \
  --project=$GOOGLE_CLOUD_PROJECT \
  --set-env-vars=GOOGLE_CLOUD_PROJECT=$GOOGLE_CLOUD_PROJECT,GOOGLE_CLOUD_REGION=$GOOGLE_CLOUD_REGION
```

### ✅ On successful deployment:

* A **Cloud Run service URL** will be provided.
* Open this URL in a browser to access the deployed **Streamlit application**.

---

## **6. Test the Application**

Choose functionality in the Streamlit UI. The application will:

* Use the **Gemini API** in **Vertex AI**.
* Display the generated responses interactively.

---

## **Conclusion**

🎉 **You have successfully:**

* Cloned and configured a Streamlit app.
* Built and containerized it with Docker.
* Deployed it using **Cloud Run** integrated with **Vertex AI Gemini**.

---

## **Additional Resources**

* [🔗 Gemini Overview](https://cloud.google.com/vertex-ai/docs/generative-ai/overview)
* [📘 Generative AI on Vertex AI Documentation](https://cloud.google.com/vertex-ai/docs/generative-ai)
* [🎥 Generative AI on YouTube](https://www.youtube.com/@GoogleCloudPlatform)
* [🍳 Vertex AI Cookbook](https://cloud.google.com/vertex-ai/docs/generative-ai/learn/cookbook)
* [📂 Google Cloud Generative AI GitHub Repository](https://github.com/GoogleCloudPlatform/generative-ai)
* [🎓 Google Cloud Training & Certification](https://cloud.google.com/training)

---

> **Last Updated:** June 23, 2025
> **Lab Last Tested:** June 23, 2025
> **© 2025 Google LLC**

Let me know if you’d like a downloadable version (PDF/Markdown/Docx) of these notes.
