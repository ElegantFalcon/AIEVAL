
# AIEVAL: AI-Powered Answer Sheet and Assignment Evaluation

## Table of Contents

  - [About](https://www.google.com/search?q=%23about)
  - [Features](https://www.google.com/search?q=%23features)
  - [Architecture](https://www.google.com/search?q=%23architecture)
  - [Technologies Used](https://www.google.com/search?q=%23technologies-used)
  - [Getting Started](https://www.google.com/search?q=%23getting-started)
      - [Prerequisites](https://www.google.com/search?q=%23prerequisites)
      - [Installation](https://www.google.com/search?q=%23installation)
      - [API Keys](https://www.google.com/search?q=%23api-keys)
  - [Usage](https://www.google.com/search?q=%23usage)
  - [How it Works](https://www.google.com/search?q=%23how-it-works)

## About

AIEVAL is an innovative full-stack application designed to automate and streamline the evaluation of answer sheets and assignments. By leveraging cutting-edge AI technologies for text extraction and content similarity scoring, it helps educators quickly and efficiently score student submissions against a reference answer sheet, providing valuable feedback and significantly reducing manual grading effort.

## Features

  * **Reference Answer Sheet Upload:** Teachers can easily upload a reference answer sheet in various document formats (e.g., images, PDFs).
  * **Student Answer Sheet Upload:** Students (or teachers on their behalf) can upload individual answer sheets.
  * **Automated Text Extraction:** Utilizes **Google Vision API** for highly accurate Optical Character Recognition (OCR) to extract text from uploaded images or PDF documents.
  * **Semantic Similarity Scoring:**
      * **SBERT (Sentence-BERT):** Compares the semantic similarity between student answers and reference answers using pre-trained SBERT models, providing a quantitative measure of content overlap.
      * **Gemini API:** Leverages the advanced capabilities of Google's Gemini API for more sophisticated and nuanced content comparison, enabling a deeper understanding of the answers and potentially qualitative feedback generation.
  * **Detailed Score Generation:** Generates a comprehensive score for each student's submission, indicating how closely their answers align with the reference, along with potential breakdowns.
  * **User-Friendly Interface:** Built with Next.js and ShadCN, the frontend provides a responsive and intuitive experience for uploading files, managing evaluations, and viewing results.

## Architecture

AIEVAL follows a client-server architecture with a clear separation between the frontend and the backend.

  * **Frontend (`frontend/` folder):** Developed with **Next.js**, this is the user-facing part of the application. It handles user interactions, displays information, and communicates with the backend via API calls.
  * **Backend (`backend/` folder):** Developed with **Python**, this serves as the API server. It is responsible for:
      * Receiving file uploads from the frontend.
      * Performing semantic similarity calculations using SBERT and Gemini API.
      * Exposing RESTful API endpoints for the frontend to consume.


## Technologies Used

### Frontend (`frontend/`)

  * **Next.js:** React framework for building fast and scalable web applications, supporting server-side rendering (SSR) and static site generation (SSG).
  * **React:** JavaScript library for building user interfaces.
  * **TypeScript:** For type safety and better code maintainability.
  * **Tailwind CSS:** For utility-first CSS styling.
  * **Axios/Fetch API:** For making HTTP requests to the Python backend.

### Backend (`backend/`)

  * **Python:** The core programming language for backend logic.
  * **`sentence-transformers` library:** For SBERT embeddings.
  * **`google-generativeai` library:** For interacting with Google Gemini API.
  * **Database (DynamoDB):** For persistent storage of reference answers, student submissions, and evaluation results.

## Getting Started

To get a local copy up and running, follow these simple steps.

### Prerequisites

  * **Node.js** (LTS version recommended) and **npm** or **yarn** for the Next.js frontend.
  * **Python 3.8+** and **pip** for the Python backend.

### Installation

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/ElegantFalcon/AIEVAL.git
    cd AIEVAL
    ```

2.  **Backend Setup (`backend/`)**

    ```bash
    cd backend
    python -m venv venv
    source venv/bin/activate  # On Windows: `venv\Scripts\activate`
    pip install -r requirements.txt
    ```

3.  **Frontend Setup (`frontend/`)**

    ```bash
    cd ../frontend
    npm install  
    ```

### API Keys

This project requires API keys for Google Vision and Google Gemini.

1.  **Google Cloud Project:**

      * Create a Google Cloud Project if you don't have one.
      * Enable the **Google Vision API** and **Gemini API** within your project.
      * **For Google Vision:** Create a service account key (JSON file) and download it.
      * **For Gemini API:** Generate an API key.

2.  **Environment Variables:**

      * **Backend (`backend/.env`):** Create a `.env` file in the `backend` directory.
        ```
        GOOGLE_APPLICATION_CREDENTIALS=/path/to/your/google-cloud-service-account.json
        GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
        ```
        (Make sure your backend code uses a library like `python-dotenv` to load these variables.)
      * **Frontend (`frontend/.env.local`):** Create a `.env.local` file in the `frontend` directory.
        ```
        NEXT_PUBLIC_BACKEND_URL=http://localhost:5000  
        ```
        (Next.js automatically loads `.env.local` variables prefixed with `NEXT_PUBLIC_`.)

## Usage

1.  **Start the Backend Server (`backend/`)**
    From the `backend` directory:

    ```bash
    source venv/bin/activate
    `uvicorn main:app --reload` 
    ```

    The backend server will typically run on `http://localhost:5000` (or another configured port).

2.  **Start the Frontend Development Server (`frontend/`)**
    From the `frontend` directory:

    ```bash
    npm run dev 
    ```

    The Next.js application will typically be accessible via `http://localhost:3000`.

3.  **Interact with AIEVAL:**

      * Open your browser and navigate to `http://localhost:3000`.
      * Use the interface to upload reference answer sheets and student submissions.
      * Initiate the evaluation process through the UI.
      * View the generated scores and feedback on the frontend.

## How it Works

1.  **User Interaction (Next.js Frontend):** The user uploads documents (images/PDFs) through the web interface.
2.  **File Upload to Backend:** The Next.js frontend sends the uploaded files as `FormData` to a designated API endpoint on the Python backend.
3.  **Text Extraction (Python Backend + Google Vision API):**
      * The Python backend receives the files.
      * It uses the Google Cloud Vision API client library to send the image/PDF data to the Google Vision service.
      * Google Vision performs OCR and returns the extracted text.
4.  **Semantic Comparison (Python Backend + SBERT/Gemini API):**
      * The extracted text from the reference answer and each student's answer is processed.
      * **SBERT:** Sentence embeddings are generated for key sections or sentences using the `sentence-transformers` library. Cosine similarity is then calculated between these embeddings to quantify the semantic overlap.
      * **Gemini API:** For more sophisticated analysis, the backend constructs prompts to the Gemini API, providing both the reference answer and the student's answer. Gemini can then generate a score, identify discrepancies, or even provide natural language feedback based on its understanding.
5.  **Score Aggregation and Storage (Python Backend):** The similarity scores (from SBERT, Gemini, or a combination, possibly weighted) are aggregated to produce a final score for each student's submission. This data, along with metadata, is then stored in the database.
6.  **Result Retrieval and Display (Next.js Frontend):** The Next.js frontend fetches the evaluation results from the Python backend's API and displays them in a user-friendly format.


-----
