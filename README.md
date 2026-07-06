# autonomous-ai-agent-design-engine

An event-driven, serverless digital asset generation pipeline that automates product-copy staging, photo-realistic image synthesis, and multi-format brand graphic rendering with 100% human-out-of-the-loop operation.

Designed as a zero-maintenance, low-cost system ($Cost_{daily} < \$0.02$), this project orchestrates n8n, Anthropic Claude 3.5 Sonnet, Flux Schnell (via Replicate), and HCTI to dynamically compile web-standard layouts on the fly.

🗺️ System Architecture

Traditional digital asset pipelines rely on brittle, templated graphic editors (e.g., Canva/Photoshop APIs) which suffer from layout breaks, clipped text, or overlapping elements when text lengths vary.

This engine solves this problem by compiling dynamic CSS Flexbox & Grid layouts on the fly, using headless browser rendering to guarantee responsive typography scaling regardless of text length.

       [ Google Sheets / CSV Source ] 
                     │
                     ▼ (Daily Schedule Trigger @ 5:00 AM NY Time)
         [ Timezone-Resilient Filter ] (JS sandboxed execution)
                     │
                     ▼ (Isolates today's record & maps dynamic keys)
         [ Claude Copy & Prompt Engine ] ◄───► [ Anthropic Claude 3.5 ]
                     │ (System-grounded Menu Portfolio execution)
                     ▼
         [ Defensive JSON Parser ] (Isolates and sanitizes payload)
                     │
         ┌───────────┴───────────┬──────────────────────────┐
         │ (Creates Folder)      │ (Async text log)         │ (Async text log)
         ▼                       ▼                          ▼
[ GDrive Directory ]    [ Caption Packer ]         [ Prompt Log Packer ]
   │ (Passes Folder ID)          │                          │
   ▼                             ▼                          ▼
[ Flux Generator ]      [ GDrive: Caption.txt ]   [ GDrive: Prompt.txt ]
   │ (Flux Schnell Predict)
   ▼
[ Headless Layout Engine ] (Compiles dynamic HTML & CSS variables)
   │
   ▼ (Async parallel rendering)
   ├──► Instagram Post (1080x1080) ──► Render HCTI ──► GDrive: /instagram.png
   ├──► Facebook Post (1080x1080)  ──► Render HCTI ──► GDrive: /fb.png
   └──► TikTok/Story (1080x1920)   ──► Render HCTI ──► GDrive: /tiktok.png


💎 Core Engineering Design Patterns

1. Dynamic UI Stencil Compilation (Decoupled Layout Rendering)

Instead of relying on rigid, hardcoded design canvas templates, we compile modular web layouts dynamically using CSS variables and HTML.

Self-Healing Typographic Scaling: By leveraging standard CSS Flexbox and container queries, long product names gracefully scale down without breaking container borders.

Aspect-Ratio Independence: The workflow compiles distinct HTML/CSS wrappers for multiple target channels (Square 1:1, Tall 9:16) asynchronously from a single upstream state.

2. Guarded AI Copy Staging via Prompt-Grounding

To eliminate LLM hallucinations and prevent the output of generic, cliché-ridden advertising copy (e.g., "Embark on a culinary journey!"), we engineered a Grounding Portfolio Context Library directly into the system instructions of the LLM.
The LLM acts as a strict compiler—cross-referencing unstructured inventory metadata against known product assets to synthesize hyper-accurate image prompts and highly targeted platform copy.

3. Defensive Serialization (The JSON Contract)

LMs frequently break structural outputs by surrounding JSON with markdown blocks (```json) or prepending text.
We implemented a sandboxed JavaScript parsing layer that utilizes regex boundaries to isolate the structural raw string, executes a safe JSON.parse(), and returns deterministic fallback state schemas if parsing fails. This acts as an elegant circuit breaker, preventing pipeline downstream crashes.

📂 Directory Structure

/workflows: Houses the master generalized generalized_marketing_workflow.json workflow file.

/config: Contains instructions for mapping your own product portfolio ("Grounding Truth").

/data: Provides sample CSV file structures for spreadsheet integration.

🚀 Quickstart & Deployment

1. Local Prototyping (Free)

You can test this entire pipeline locally on your computer with zero hosting fees:

# Start a local n8n instance using Docker
docker run -it --rm --name n8n -p 5678:5678 n8nio/n8n


Open http://localhost:5678 in your browser, create a new workflow, and import the workflows/generalized_marketing_workflow.json file.

2. Cloud Deployment (Production)

For continuous, 24/7 background triggers (configured at 5:00 AM NY Time), deploy the JSON file onto a managed cloud host such as Pikapods or a cloud VPS instance.

3. Connection Configuration

Double-click the respective nodes in n8n to connect your own developer API credentials:

Google Sheets/Drive: Authorize via standard Google OAuth.

Anthropic Chat Model: Drop in your API key (sk-ant-...).

Replicate Prediction Node: Replace with your Replicate Token under the Authorization header (Bearer r8_...).

HTMLtoImage Node: Replace the top-level credentials in the payload compiler with your HCTI User ID and API Key.

📊 Architectural Trade-offs & Decisions

Decision Metric

Hardcoded Python Stack (Lambda/EC2)

Low-Code / High-Logic Orchestration (n8n)

Operational Overhead

High (monitoring runtimes, security patches, library deprecation)

Zero (Managed serverless environments, visual execution state logging)

Development Lifecycle

Weeks (writing boilerplate API integrations, custom auth handlers)

Hours (Visual debugging of async parallel loops, native OAuth handlers)

Maintenance & Logging

Complex (setting up Datadog, CloudWatch log streams)

Excellent (Visual step-by-step canvas execution node replays)

Runtime Infrastructure Cost

$15 - $30/month (base CPU instances)

$3 - $5/month (minimal shared CPU pods)

📄 License

This project is open-source and licensed under the MIT License.
