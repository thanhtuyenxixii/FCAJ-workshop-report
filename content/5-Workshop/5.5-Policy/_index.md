---
title : "VPC Endpoint Policies"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Step 1: Confirm Amazon SNS Email Subscription

Before submitting any essays, you must approve the subscription to receive automatic score emails:

1. Log into your email account (the one provided during the stack deployment, e.g. testuser@gmail.com).
2. Find the email sent by no-reply@sns.amazonaws.com with the subject **"AWS Notification - Subscription Confirmation"**.
3. Click the **Confirm Subscription** link in the body. The browser will show **Subscription confirmed!**.

![SNS subscription confirmation](/images/5-Workshop/5.5-Policy/confirm-notification.jpeg)

---

#### Step 2: Access the Frontend Web Application

1. Open your web browser and navigate to the **CloudFront URL**.
2. Sign in using the test account credentials:
   * **Email:** testuser@gmail.com
   * **Password:** MatKhauChauAu123!

![Sign In Page Web UI](/images/5-Workshop/5.5-Policy/test-web.png)

3. You will be redirected to the **Dashboard** page.


---

#### Step 3: Test Uploading Essays

The system supports .txt files, images (.png, .jpg, .jpeg), and .pdf files. Submit them sequentially to test the pipelines:

##### Case A: Uploading a Plain Text Essay (.txt)
1. Select or drop a .txt file containing your English essay into the **Upload Essay** panel on the left.
2. Click **Submit for Assessment**. The status will become Analyzing.
3. After 30-50 seconds, the status will update to Scored and show the score.

##### Case B: Uploading an Image (.png, .jpg, .jpeg)
1. Drop an image of a handwritten or printed essay.
2. Click **Submit for Assessment**.
3. The step functions state machine will execute a **Choice State**, routing the image to **Amazon Textract** to perform OCR, parsing the blocks, and sending the extracted text to Gemini AI.

##### Case C: Uploading a Document (.pdf)
1. Submit a .pdf file.
2. Textract will scan the pages and extract the text blocks for evaluation.
![Sign In Page Web UI](/images/5-Workshop/5.5-Policy/thuc-hien-cham-diem.png)


---

#### Step 4: Verify Evaluation Results and Emails

##### 1. View Detailed Reports on the UI:
* Click the **View Analysis** button on any scored essay.
* The system displays:
  * **Overall Score:** out of 100.
  * **Overall Feedback:** Advice and comments from the AI.
  * **Evaluation Criteria:** Score breakdown for Grammar, Vocabulary, Structure, and Coherence.
![Dashboard Page UI](/images/5-Workshop/5.5-Policy/ket-qua-cham-diem.png)

##### 2. Check your Email Inbox:
* Open your inbox and look for an email notification:
  * **Subject:** Essay Scored! File: [Your_Filename]
* The body contains the score and the evaluation report.

![Evaluation Detailed Report page](/images/5-Workshop/5.5-Policy/mail-thong-bao.png)


