# Design Document: AP Chatbot Survey System

## Overview

The AP Chatbot Survey System is a client-side web application designed to collect qualitative research data about student interactions with AI chatbots exhibiting different communication styles. The system presents three sequential chatbot experiences, each with a distinct personality (sarcastic, friendly, and casual teen), enabling comparative analysis of user preferences and engagement patterns for a high school AP research project.

### System Characteristics

- **Architecture**: Three standalone HTML pages with embedded CSS and JavaScript
- **Deployment**: Static file hosting (no backend infrastructure required)
- **State Management**: In-memory, session-scoped (no persistence across page loads)
- **Navigation**: Sequential flow with direct HTML file navigation
- **Data Collection**: No automated data capture; qualitative feedback collected via in-person survey

### Design Philosophy

The system prioritizes simplicity and consistency. By using identical visual design and interaction patterns across all three chatbots, the system ensures that participants focus on personality differences rather than interface variations. The fixed question set and sequential presentation order enable controlled comparison across all participants.

## Architecture

### High-Level Structure

```
┌─────────────────────────────────────────────────────────────┐
│                    Static File Hosting                       │
└─────────────────────────────────────────────────────────────┘
                              │
                ┌─────────────┼─────────────┐
                │             │             │
         ┌──────▼──────┐ ┌───▼────────┐ ┌─▼──────────────┐
         │  index.html │ │ chatbot-   │ │ chatbot-       │
         │ (Sarcastic) │ │ friendly.  │ │ teen.html      │
         │             │ │ html       │ │ (Teen)         │
         │  Bot #1     │ │ (Friendly) │ │  Bot #3        │
         │             │ │  Bot #2    │ │                │
         └──────┬──────┘ └───┬────────┘ └─┬──────────────┘
                │            │             │
                └────────────┼─────────────┘
                             │
                    Sequential Navigation
                    (window.location.href)
```

### Component Architecture

Each chatbot page is a self-contained unit with the following embedded components:

1. **Presentation Layer** (HTML + CSS)
   - Header component (title and subtitle)
   - Chat container (scrollable message display)
   - Input section (question buttons and navigation)

2. **Interaction Layer** (JavaScript)
   - Message rendering system
   - State management (askedQuestions Set)
   - Event handlers (button clicks)
   - Animation timing controller
   - Completion detection logic

3. **Data Layer** (JavaScript constants)
   - Response library (personality-specific content)
   - Question set definition
   - Timing configuration

### Separation of Concerns

Despite being monolithic HTML files, the implementation maintains clear separation:

- **Structure** (HTML): Defines semantic layout and initial content
- **Presentation** (CSS): Handles all visual styling, animations, and responsive behavior
- **Behavior** (JavaScript): Manages interaction logic, state, and dynamic content

## Components and Interfaces

### 1. Chat Container Component

**Purpose**: Displays conversation history with automatic scrolling

**Structure**:
```html
<div class="chat-container" id="chatContainer">
  <div class="message ai|user">
    <div class="label">AI|You</div>
    <div class="message-content">[text]</div>
  </div>
</div>
```

**Styling**:
- Fixed height: 400px
- Vertical scrolling: enabled (overflow-y: auto)
- Background: #f5f5f5
- Padding: 20px

**Behavior**:
- Auto-scrolls to bottom on new message (scrollTop = scrollHeight)
- Preserves manual scroll position between message additions
- Applies fade-in animation to new messages

### 2. Message Rendering System

**Function Signature**:
```javascript
function addMessage(text: string, isUser: boolean): void
```

**Implementation**:
- Creates message DOM elements dynamically
- Applies appropriate CSS classes based on message type
- Appends to chat container
- Triggers auto-scroll

**Message Types**:
- **AI Messages**: Left-aligned, white background, gray border
- **User Messages**: Right-aligned, purple gradient background, white text

### 3. Question Button Grid

**Purpose**: Presents available questions as interactive buttons

**Structure**:
```html
<div class="options-grid" id="optionsGrid">
  <button class="option-button" 
          data-key="[UPPERCASE_KEY]" 
          onclick="sendMessage('[UPPERCASE_KEY]')">
    [Display Text]
  </button>
</div>
```

**Styling**:
- Layout: CSS Grid with 10px gap
- Hover effects: Border color change, upward translation (-2px)
- Active state: Reset translation
- Cursor: pointer

**Behavior**:
- Buttons hidden via `display: none` after selection
- Data attribute stores uppercase key for state tracking

### 4. State Management System

**Data Structure**:
```javascript
let askedQuestions = new Set<string>();
const totalQuestions = 4;
```

**Purpose**: Tracks which questions have been asked in current session

**Operations**:
- **Add**: `askedQuestions.add(message)` when question selected
- **Check**: `askedQuestions.size === totalQuestions` for completion
- **Reset**: Automatic on page load (no persistence)

**Scope**: Per-page session (state does not transfer between chatbots)

### 5. Response Library

**Data Structure**:
```javascript
const responses = {
  "WHAT IS THE WEATHER LIKE TODAY": [string, string],
  "HOW ARE YOU": [string, string],
  "GIVE ME AN UPDATE ON THE NEWS": [string, string],
  "SOLVE THIS EQUATION": [string, string]
};
```

**Characteristics**:
- Each question maps to exactly 2 response strings
- Responses are personality-specific per chatbot
- Accessed via uppercase key matching

**Personality Variations**:
- **Sarcastic Bot**: Condescending, darkly humorous tone
- **Friendly Bot**: Enthusiastic, supportive tone
- **Teen Bot**: Casual, laid-back tone with lowercase text

### 6. Animation System

**Message Animation**:
```css
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
```

**Timing Configuration**:
- User message: Immediate display
- First AI response: 500ms delay
- Subsequent AI responses: 800ms delay between each
- Farewell messages: 1000ms delays between each
- UI state changes: 1000ms delay after final message

**Implementation**: JavaScript `setTimeout` with nested callbacks

### 7. Completion Detection System

**Function**:
```javascript
function checkIfComplete(): void
```

**Logic**:
1. Check if `askedQuestions.size === totalQuestions`
2. If true, display farewell messages with timing delays
3. Hide question selection interface
4. Show navigation button (or final message for Teen Bot)

**Trigger**: Called after each AI response sequence completes

### 8. Navigation System

**Mechanism**: Direct HTML file navigation via `window.location.href`

**Flow**:
- Sarcastic Bot → `chatbot-friendly.html`
- Friendly Bot → `chatbot-teen.html`
- Teen Bot → No navigation (displays survey instructions)

**Button Display Logic**:
- Navigation buttons initially hidden (`display: none`)
- Shown only after conversation completion
- Teen Bot shows final message instead of navigation button

## Data Models

### Question Model

```typescript
interface Question {
  key: string;           // Uppercase identifier (e.g., "HOW ARE YOU")
  displayText: string;   // User-facing text (e.g., "How are you")
  responses: {
    sarcastic: [string, string];
    friendly: [string, string];
    teen: [string, string];
  };
}
```

**Fixed Question Set**:
1. "WHAT IS THE WEATHER LIKE TODAY"
2. "HOW ARE YOU"
3. "GIVE ME AN UPDATE ON THE NEWS"
4. "SOLVE THIS EQUATION"

### Message Model

```typescript
interface Message {
  text: string;
  isUser: boolean;
  timestamp?: Date;  // Not implemented in current system
}
```

**Rendering Properties**:
- User messages: Right-aligned, purple gradient background
- AI messages: Left-aligned, white background with gray border

### Chatbot Configuration Model

```typescript
interface ChatbotConfig {
  name: string;
  mode: string;
  greetings: [string, string];
  responses: Record<string, [string, string]>;
  farewells: [string, string];
  nextPage: string | null;
  textTransform?: 'lowercase';
}
```

**Instances**:
1. **Sarcastic Bot** (index.html)
   - Mode: "Limited Interaction Mode"
   - Next: "chatbot-friendly.html"
   
2. **Friendly Bot** (chatbot-friendly.html)
   - Mode: "Friendly Mode"
   - Next: "chatbot-teen.html"
   
3. **Teen Bot** (chatbot-teen.html)
   - Mode: "Chill Mode"
   - Next: null
   - Text Transform: lowercase

### Session State Model

```typescript
interface SessionState {
  askedQuestions: Set<string>;
  totalQuestions: number;
  isComplete: boolean;  // Derived from askedQuestions.size
}
```

**Lifecycle**:
- **Initialization**: Empty Set on page load
- **Updates**: Questions added as user interacts
- **Termination**: State discarded on page navigation (no persistence)

## Design System

### Color Palette

- **Primary Gradient**: `linear-gradient(135deg, #667eea 0%, #764ba2 100%)`
- **Background**: Applied to body, header, user messages, buttons
- **Neutral Colors**:
  - White: `#ffffff` (container, AI messages)
  - Light Gray: `#f5f5f5` (chat background)
  - Border Gray: `#e0e0e0` (borders, inactive buttons)
  - Text: `#333333` (primary text)

### Typography

- **Font Family**: System font stack
  ```css
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 
               Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  ```
- **Sizes**:
  - Header title: 24px
  - Header subtitle: 14px
  - Message content: 14px (default)
  - Labels: 12px
  - Buttons: 14px

### Spacing System

- **Container padding**: 20px
- **Message margin**: 15px bottom
- **Button gap**: 10px (grid)
- **Message padding**: 12px vertical, 16px horizontal
- **Border radius**: 6px (buttons), 12px (container), 18px (messages)

### Responsive Behavior

- **Container max-width**: 700px
- **Message max-width**: 80% of container
- **Viewport support**: Minimum 320px width
- **Layout**: Flexbox and CSS Grid for responsive adaptation

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

### Property Reflection

After analyzing all acceptance criteria, several redundancies were identified:

- **6.5 and 7.5**: Both test fade-in animations on messages (consolidated into Property 5)
- **3.1 and 3.2**: Question hiding and visibility are inverse operations (consolidated into Property 2)
- **11.1 and 11.2**: Both test Teen Bot lowercase styling (consolidated into example tests)

The following properties represent the unique, testable behaviors of the system:

### Property 1: Question Set Consistency

*For any* chatbot page in the system, the question set shall contain exactly four questions with the keys "WHAT IS THE WEATHER LIKE TODAY", "HOW ARE YOU", "GIVE ME AN UPDATE ON THE NEWS", and "SOLVE THIS EQUATION", and these questions shall appear in the same order across all pages.

**Validates: Requirements 2.1, 2.2, 2.4**

### Property 2: Question Button State Management

*For any* question button on any chatbot page, when the button is clicked, it shall be hidden from the interface, and all unclicked buttons shall remain visible until they are clicked.

**Validates: Requirements 3.1, 3.2**

### Property 3: Session State Isolation

*For any* chatbot page, when the page loads, all four question buttons shall be visible regardless of which questions were asked on previous pages.

**Validates: Requirements 3.3**

### Property 4: Response Structure Consistency

*For any* question in the response library on any chatbot page, the question shall map to exactly two response strings.

**Validates: Requirements 4.4**

### Property 5: Message Animation Application

*For any* message added to the chat container, the message element shall have the fadeIn animation class applied.

**Validates: Requirements 6.5, 7.5**

### Property 6: Completion Detection

*For any* chatbot page, when all four questions have been asked, the system shall display farewell messages, hide the question selection interface, and show either a navigation button or final instructions.

**Validates: Requirements 5.1, 5.2**

### Property 7: Visual Consistency Across Pages

*For any* chatbot page, the page shall use the purple gradient color scheme (#667eea to #764ba2), identical DOM structure for layout components, and consistent message bubble styling.

**Validates: Requirements 6.1, 6.2, 6.3**

### Property 8: Message Type Differentiation

*For any* message in the chat container, user messages shall have different CSS classes and visual styling than AI messages.

**Validates: Requirements 6.4**

### Property 9: Response Timing Sequence

*For any* question with multiple responses, the first AI response shall appear 500ms after the user message, and each subsequent response shall appear 800ms after the previous response.

**Validates: Requirements 7.2, 7.3, 4.5**

### Property 10: Auto-Scroll Behavior

*For any* message added to the chat container, the container shall automatically scroll to show the most recent message by setting scrollTop to scrollHeight.

**Validates: Requirements 7.4, 15.3**

### Property 11: Message Bubble Width Constraint

*For any* message content element, the maximum width shall be 80% of the container width.

**Validates: Requirements 8.3**

### Property 12: Client-Side Only Implementation

*For any* JavaScript code in the system, there shall be no network requests (fetch, XMLHttpRequest, WebSocket) and no data persistence mechanisms (localStorage, sessionStorage, cookies, IndexedDB).

**Validates: Requirements 9.2, 9.4**

### Property 13: Navigation Button Visibility

*For any* chatbot page, the navigation button shall be hidden (display: none) until all four questions have been asked.

**Validates: Requirements 10.1**

### Property 14: Direct File Navigation

*For any* navigation button, the onclick handler shall use window.location.href with a direct HTML file path.

**Validates: Requirements 10.5**

### Property 15: Initial Greeting Messages

*For any* chatbot page, when the page loads, exactly two AI greeting messages shall be present in the chat container before any user interaction.

**Validates: Requirements 13.1, 13.5**

### Property 16: Button Hover Effects

*For any* question button, the CSS shall include hover pseudo-class styling with border color change to #667eea and upward transform translation.

**Validates: Requirements 14.1, 14.2, 14.3**

### Property 17: Button Interaction Styling

*For any* question button, the CSS shall include active pseudo-class styling and cursor: pointer.

**Validates: Requirements 14.4, 14.5**

### Property 18: Chat Container Constraints

*For any* chatbot page, the chat container shall have a height of 400 pixels and overflow-y set to auto or scroll.

**Validates: Requirements 15.1, 15.2**

### Property 19: Teen Bot Lowercase Responses

*For any* response in the Teen Bot response library, the text shall be entirely lowercase.

**Validates: Requirements 4.3 (partial)**

### Property 20: Question Tracking Accuracy

*For any* chatbot page session, the askedQuestions Set shall contain exactly the questions that have been clicked, and the size of the Set shall equal the number of hidden question buttons.

**Validates: Requirements 3.4**

## Error Handling

### Error Categories

The system has minimal error handling requirements due to its constrained, static nature:

1. **User Input Errors**: Not applicable (no free-form input)
2. **Network Errors**: Not applicable (no network requests)
3. **Data Validation Errors**: Not applicable (fixed question set)
4. **State Corruption**: Prevented by page reload resetting state

### Defensive Programming Practices

Despite the simple architecture, the following practices ensure robustness:

1. **DOM Element Existence**: All DOM queries use `getElementById` with known IDs
2. **Data Key Matching**: Uppercase normalization ensures consistent key lookup
3. **Timing Coordination**: Nested setTimeout ensures sequential execution
4. **CSS Fallbacks**: System font stack provides cross-platform compatibility

### Graceful Degradation

- **JavaScript Disabled**: System non-functional (acceptable for research context)
- **CSS Not Loaded**: Content remains accessible but unstyled
- **Small Viewports**: Responsive design maintains usability down to 320px

## Testing Strategy

### Dual Testing Approach

The system requires both unit testing and property-based testing for comprehensive coverage:

- **Unit Tests**: Verify specific examples, edge cases, and configuration details
- **Property Tests**: Verify universal properties across all inputs and pages

Together, these approaches ensure both concrete correctness (unit tests catch specific bugs) and general correctness (property tests verify behavior holds across all scenarios).

### Unit Testing Focus Areas

Unit tests should focus on:

1. **Specific Configuration Examples**
   - Sarcastic Bot navigates to Friendly Bot (Req 1.2)
   - Friendly Bot navigates to Teen Bot (Req 1.3)
   - Teen Bot displays survey instructions (Req 1.5)
   - Teen Bot has no navigation button (Req 1.4)

2. **Personality-Specific Behavior**
   - Teen Bot applies lowercase CSS transform (Req 11.1, 11.2, 11.4)
   - Teen Bot displays final message box (Req 12.1)
   - Final message contains "Phoebe" and "survey" (Req 12.2, 12.3)
   - Final message has purple accent styling (Req 12.4)

3. **Edge Cases**
   - Mobile viewport at 320px width (Req 8.2)
   - Initial page load state (Req 1.1)
   - Static deployment verification (Req 9.1)

4. **Integration Points**
   - Button click triggers correct message flow
   - Completion detection triggers correct UI changes
   - Navigation buttons link to correct pages

### Property-Based Testing Configuration

**Library Selection**: 
- **JavaScript**: Use `fast-check` library for property-based testing
- Installation: `npm install --save-dev fast-check`

**Test Configuration**:
- Minimum 100 iterations per property test
- Each test must reference its design document property
- Tag format: `// Feature: ap-chatbot-survey, Property {number}: {property_text}`

**Example Property Test Structure**:
```javascript
const fc = require('fast-check');

// Feature: ap-chatbot-survey, Property 1: Question Set Consistency
test('all chatbot pages have identical question sets', () => {
  fc.assert(
    fc.property(
      fc.constantFrom('index.html', 'chatbot-friendly.html', 'chatbot-teen.html'),
      (page) => {
        const questions = extractQuestions(page);
        return questions.length === 4 &&
               questions.includes('WHAT IS THE WEATHER LIKE TODAY') &&
               questions.includes('HOW ARE YOU') &&
               questions.includes('GIVE ME AN UPDATE ON THE NEWS') &&
               questions.includes('SOLVE THIS EQUATION');
      }
    ),
    { numRuns: 100 }
  );
});
```

### Property Test Coverage

Each of the 20 correctness properties must be implemented as a single property-based test:

1. **Property 1**: Generate arbitrary page selections, verify question set
2. **Property 2**: Generate arbitrary button click sequences, verify visibility
3. **Property 3**: Simulate page loads, verify initial state
4. **Property 4**: Generate arbitrary question keys, verify response array length
5. **Property 5**: Generate arbitrary messages, verify animation class
6. **Property 6**: Generate arbitrary question sequences, verify completion behavior
7. **Property 7**: Generate arbitrary page selections, verify visual consistency
8. **Property 8**: Generate arbitrary messages, verify type differentiation
9. **Property 9**: Generate arbitrary questions, verify timing values
10. **Property 10**: Generate arbitrary messages, verify scroll behavior
11. **Property 11**: Parse CSS, verify max-width constraint
12. **Property 12**: Parse JavaScript, verify no network/storage calls
13. **Property 13**: Generate arbitrary completion states, verify button visibility
14. **Property 14**: Parse navigation handlers, verify window.location.href usage
15. **Property 15**: Generate arbitrary page loads, verify greeting count
16. **Property 16**: Parse CSS, verify hover pseudo-class properties
17. **Property 17**: Parse CSS, verify active pseudo-class and cursor
18. **Property 18**: Parse CSS, verify container height and overflow
19. **Property 19**: Generate arbitrary Teen Bot responses, verify lowercase
20. **Property 20**: Generate arbitrary click sequences, verify Set accuracy

### Testing Tools

- **Test Runner**: Jest or Mocha
- **Property Testing**: fast-check
- **DOM Testing**: jsdom for simulating browser environment
- **CSS Parsing**: css-tree or postcss for style verification
- **HTML Parsing**: cheerio or jsdom for structure verification

### Test Organization

```
tests/
├── unit/
│   ├── navigation.test.js
│   ├── personality.test.js
│   ├── completion.test.js
│   └── responsive.test.js
└── properties/
    ├── question-set.property.test.js
    ├── state-management.property.test.js
    ├── visual-consistency.property.test.js
    ├── timing.property.test.js
    └── styling.property.test.js
```

### Manual Testing Checklist

Due to the visual and interactive nature of the system, manual testing should verify:

- [ ] Visual appearance matches design system across all three pages
- [ ] Animations feel smooth and natural
- [ ] Hover effects provide clear feedback
- [ ] Mobile experience is usable on actual devices
- [ ] Personality differences are immediately apparent
- [ ] Navigation flow is intuitive
- [ ] Survey instructions are clear and actionable

### Acceptance Testing

Final acceptance testing should involve actual AP research participants:

1. Observe 3-5 students completing the full chatbot sequence
2. Verify they understand the navigation flow
3. Confirm they can complete all interactions without assistance
4. Validate that personality differences are noticeable
5. Ensure survey instructions are clear

This real-world validation ensures the system meets its research objectives beyond technical correctness.
