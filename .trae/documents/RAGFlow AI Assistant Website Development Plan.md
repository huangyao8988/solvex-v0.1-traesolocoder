I have analyzed the requirements and the RAGFlow API documentation. Here is the comprehensive plan to develop the RAGFlow-based AI Assistant Website.

## 1. Project Architecture
The solution will be a mono-repo containing two main parts:
- **Backend (`/backend`)**: Spring Boot 3.5 (Java 21) application serving as the API gateway, session manager, and RAGFlow proxy.
- **Frontend (`/frontend`)**: Vue 3 application providing the conversational UI.

### Technology Stack
- **Backend**: Spring Boot 3.5, JDK 21, Spring Web, Spring WebSocket, Spring Data JPA, H2 Database (for user/session persistence), Lombok.
- **Frontend**: Vue 3, Vite, Pinia (State Management), Tailwind CSS (UI), Markdown-it (Rendering), Axios.

## 2. Implementation Steps

### Phase 1: Backend Setup (Spring Boot)
1.  **Project Initialization**: Create standard Spring Boot structure.
2.  **RAGFlow Integration**:
    -   Implement `RagFlowClient` to interact with `http://ragflow01-api:80`.
    -   Endpoints to implement:
        -   `GET /api/v1/chats` (List assistants - to select one automatically)
        -   `POST /api/v1/chats/{chat_id}/sessions` (Create session)
        -   `POST /api/v1/chats/{chat_id}/completions` (Send message)
3.  **Authentication & User Management**:
    -   Implement simple JWT-based Auth.
    -   Support "Guest Mode" (generate UUID for anonymous users).
    -   Persist Users and Sessions in H2 Database.
4.  **Real-time Messaging**:
    -   Implement WebSocket (`/ws/chat`) to stream RAGFlow responses to the frontend.
    -   Handle parsing of RAGFlow stream chunks and forwarding them.

### Phase 2: Frontend Setup (Vue 3)
1.  **Project Initialization**: Create Vue 3 project with Vite.
2.  **UI Components**:
    -   **ChatWindow**: Main conversation area.
    -   **MessageBubble**: Renders User/AI messages.
        -   **Citation Handling**: Parse `##Index$$` markers in AI response and render them as clickable citations.
        -   **Popover**: Show source text (highlighted) when citation is clicked.
    -   **Sidebar**: List historical sessions (for logged-in users).
    -   **AuthModal**: Login/Register forms.
    -   **AdBanner**: Reserved ad slots.
3.  **State Management**: Use Pinia to manage current session, messages, and auth state.
4.  **API Integration**: Connect to Spring Boot backend for Chat and Auth.

### Phase 3: Integration & Styling
1.  **Theming**: Apply the color scheme based on the provided logo path (Blue/Tech theme).
2.  **Verification**:
    -   Verify Guest vs. Logged-in behavior.
    -   Verify Citation popup accuracy.
    -   Verify Real-time streaming performance.

## 3. Key Features Detail
-   **Citations**: The backend will forward the `reference` object from RAGFlow. The frontend will parse the answer text, replace citation markers with interactive badges, and display the corresponding chunk content on hover/click.
-   **Guest Access**: Guests will have a temporary session stored in LocalStorage. Upon login, we can optionally merge or just start fresh (simplest is separate).
