# Requirements Document

## Introduction

This document specifies the requirements for an AP Research Survey Chatbot System designed to collect qualitative data about student interactions with AI chatbots exhibiting different communication styles. The system presents three sequential chatbot experiences with distinct personalities (sarcastic, friendly, and casual teen) to enable comparative analysis of user preferences and engagement patterns for a high school AP research project.

## Glossary

- **Survey_System**: The complete web-based application consisting of three chatbot interfaces and navigation flow
- **Chatbot_Interface**: A single interactive web page presenting one AI personality
- **Sarcastic_Bot**: The first chatbot with condescending, dark humor personality (index.html)
- **Friendly_Bot**: The second chatbot with enthusiastic, helpful personality (chatbot-friendly.html)
- **Teen_Bot**: The third chatbot with casual, laid-back teenage personality (chatbot-teen.html)
- **Question_Set**: The fixed collection of four questions available to users: weather, how are you, news, and solve equation
- **Conversation_Session**: A single user's interaction with one chatbot from start to completion
- **Response_Library**: The predefined personality-specific answers for each question
- **Survey_Participant**: A high school student interacting with the chatbot system
- **Research_Administrator**: The AP student (Phoebe) conducting the research study

## Requirements

### Requirement 1: Sequential Chatbot Presentation

**User Story:** As a Research_Administrator, I want Survey_Participants to experience all three chatbot personalities in a specific order, so that I can collect consistent comparative data across all participants.

#### Acceptance Criteria

1. THE Survey_System SHALL present the Sarcastic_Bot as the first chatbot experience
2. WHEN a Survey_Participant completes interaction with the Sarcastic_Bot, THE Survey_System SHALL provide navigation to the Friendly_Bot
3. WHEN a Survey_Participant completes interaction with the Friendly_Bot, THE Survey_System SHALL provide navigation to the Teen_Bot
4. THE Teen_Bot SHALL be the final chatbot in the sequence
5. WHEN a Survey_Participant completes interaction with the Teen_Bot, THE Survey_System SHALL display instructions to see the Research_Administrator for a survey

### Requirement 2: Fixed Question Set

**User Story:** As a Research_Administrator, I want all Survey_Participants to have access to the same four questions across all chatbots, so that I can compare responses to identical prompts across different communication styles.

#### Acceptance Criteria

1. THE Question_Set SHALL contain exactly four questions: "What is the weather like today", "How are you", "Give me an update on the news", and "Solve this equation"
2. THE Survey_System SHALL present the identical Question_Set in all three Chatbot_Interfaces
3. THE Survey_System SHALL display questions as selectable buttons
4. THE Survey_System SHALL present questions in consistent order across all Chatbot_Interfaces

### Requirement 3: Question Availability Management

**User Story:** As a Survey_Participant, I want each question to be available only once per chatbot, so that I experience a natural conversation flow without repetition.

#### Acceptance Criteria

1. WHEN a Survey_Participant selects a question, THE Chatbot_Interface SHALL remove that question button from the available options
2. THE Chatbot_Interface SHALL maintain visibility of unasked questions until they are selected
3. WHEN a Survey_Participant navigates to a new Chatbot_Interface, THE Survey_System SHALL reset question availability to show all four questions
4. THE Chatbot_Interface SHALL track which questions have been asked within the current Conversation_Session

### Requirement 4: Personality-Specific Responses

**User Story:** As a Research_Administrator, I want each chatbot to respond with personality-consistent answers, so that Survey_Participants experience distinct communication styles for research comparison.

#### Acceptance Criteria

1. WHEN a Survey_Participant asks a question to the Sarcastic_Bot, THE Sarcastic_Bot SHALL respond with condescending and darkly humorous language
2. WHEN a Survey_Participant asks a question to the Friendly_Bot, THE Friendly_Bot SHALL respond with enthusiastic and supportive language
3. WHEN a Survey_Participant asks a question to the Teen_Bot, THE Teen_Bot SHALL respond with casual and laid-back language using lowercase text
4. THE Response_Library SHALL contain two response messages for each question per chatbot personality
5. THE Chatbot_Interface SHALL display response messages sequentially with timing delays between messages

### Requirement 5: Conversation Completion Detection

**User Story:** As a Survey_Participant, I want to know when I have completed all available questions with a chatbot, so that I understand when to proceed to the next chatbot.

#### Acceptance Criteria

1. WHEN a Survey_Participant has asked all four questions in a Conversation_Session, THE Chatbot_Interface SHALL display a personality-appropriate farewell message
2. THE Chatbot_Interface SHALL hide the question selection interface after all questions are asked
3. WHERE the Chatbot_Interface is not the Teen_Bot, THE Chatbot_Interface SHALL display a "Continue to Next Chatbot" button after conversation completion
4. WHERE the Chatbot_Interface is the Teen_Bot, THE Chatbot_Interface SHALL display survey instructions after conversation completion

### Requirement 6: Visual Consistency

**User Story:** As a Research_Administrator, I want all three chatbots to share consistent visual design, so that Survey_Participants focus on personality differences rather than interface differences.

#### Acceptance Criteria

1. THE Survey_System SHALL apply a purple gradient color scheme (hex #667eea to #764ba2) across all Chatbot_Interfaces
2. THE Survey_System SHALL use identical layout structure for all three Chatbot_Interfaces
3. THE Survey_System SHALL display messages in chat bubble format with consistent styling
4. THE Survey_System SHALL distinguish user messages from AI messages through visual styling
5. THE Survey_System SHALL apply smooth fade-in animations to new messages

### Requirement 7: Message Display Behavior

**User Story:** As a Survey_Participant, I want to see a natural conversation flow with realistic timing, so that the chatbot interaction feels authentic.

#### Acceptance Criteria

1. WHEN a Survey_Participant selects a question, THE Chatbot_Interface SHALL display the user's message immediately
2. THE Chatbot_Interface SHALL delay the first AI response by 500 milliseconds after the user message
3. WHERE multiple AI response messages exist for a question, THE Chatbot_Interface SHALL delay each subsequent message by 800 milliseconds
4. THE Chatbot_Interface SHALL automatically scroll to show the most recent message
5. THE Chatbot_Interface SHALL apply fade-in animation to each new message

### Requirement 8: Responsive Design

**User Story:** As a Survey_Participant, I want to interact with the chatbots on any device, so that I can complete the survey on my phone, tablet, or computer.

#### Acceptance Criteria

1. THE Survey_System SHALL adapt layout to viewport width using responsive CSS
2. THE Survey_System SHALL maintain readability on mobile devices with minimum 320px width
3. THE Survey_System SHALL scale message bubbles to maximum 80% of container width
4. THE Survey_System SHALL maintain touch-friendly button sizes on mobile devices

### Requirement 9: Static Deployment

**User Story:** As a Research_Administrator, I want to deploy the Survey_System without server infrastructure, so that I can host it freely and reliably for my AP research project.

#### Acceptance Criteria

1. THE Survey_System SHALL function entirely with client-side HTML, CSS, and JavaScript
2. THE Survey_System SHALL require no backend server or database
3. THE Survey_System SHALL be deployable to static hosting services
4. THE Survey_System SHALL store no user data or conversation history

### Requirement 10: Navigation Flow Control

**User Story:** As a Research_Administrator, I want Survey_Participants to complete chatbots in sequence without skipping, so that all participants have equivalent experiences.

#### Acceptance Criteria

1. THE Survey_System SHALL provide navigation buttons only after conversation completion
2. THE Sarcastic_Bot SHALL navigate to the Friendly_Bot when the continue button is clicked
3. THE Friendly_Bot SHALL navigate to the Teen_Bot when the continue button is clicked
4. THE Teen_Bot SHALL provide no navigation button after completion
5. THE Survey_System SHALL use direct HTML file navigation without routing logic

### Requirement 11: Personality-Specific Text Styling

**User Story:** As a Research_Administrator, I want the Teen_Bot to visually reinforce its casual personality through text presentation, so that the personality distinction is immediately apparent.

#### Acceptance Criteria

1. WHERE the Chatbot_Interface is the Teen_Bot, THE Chatbot_Interface SHALL display all text in lowercase
2. WHERE the Chatbot_Interface is the Teen_Bot, THE Chatbot_Interface SHALL apply lowercase styling to headers, labels, and messages
3. WHERE the Chatbot_Interface is not the Teen_Bot, THE Chatbot_Interface SHALL use standard capitalization
4. THE Teen_Bot SHALL apply lowercase transformation through CSS styling

### Requirement 12: Survey Completion Instructions

**User Story:** As a Research_Administrator, I want Survey_Participants to know how to complete the survey after finishing all chatbots, so that I can collect their feedback data.

#### Acceptance Criteria

1. WHEN a Survey_Participant completes the Teen_Bot conversation, THE Teen_Bot SHALL display a final message box
2. THE final message SHALL instruct Survey_Participants to see Phoebe for a survey
3. THE final message SHALL thank Survey_Participants for interacting with all three chatbots
4. THE final message SHALL use prominent visual styling with purple accent colors

### Requirement 13: Initial Greeting Messages

**User Story:** As a Survey_Participant, I want each chatbot to greet me with personality-appropriate messages, so that I immediately understand the chatbot's communication style.

#### Acceptance Criteria

1. WHEN a Chatbot_Interface loads, THE Chatbot_Interface SHALL display two initial greeting messages
2. THE Sarcastic_Bot SHALL display greeting messages with condescending tone
3. THE Friendly_Bot SHALL display greeting messages with welcoming tone
4. THE Teen_Bot SHALL display greeting messages with casual tone
5. THE Chatbot_Interface SHALL display greeting messages before any user interaction

### Requirement 14: Question Button Interaction

**User Story:** As a Survey_Participant, I want clear visual feedback when interacting with question buttons, so that I understand which questions are clickable.

#### Acceptance Criteria

1. WHEN a Survey_Participant hovers over a question button, THE Chatbot_Interface SHALL apply visual hover effects
2. THE hover effect SHALL include border color change to purple (#667eea)
3. THE hover effect SHALL include subtle upward translation animation
4. WHEN a Survey_Participant clicks a question button, THE Chatbot_Interface SHALL apply active state styling
5. THE Chatbot_Interface SHALL use pointer cursor for question buttons

### Requirement 15: Chat Container Scrolling

**User Story:** As a Survey_Participant, I want the conversation to remain visible as messages accumulate, so that I can review the conversation history.

#### Acceptance Criteria

1. THE Chatbot_Interface SHALL constrain the chat display area to 400 pixels height
2. WHEN messages exceed the visible area, THE Chatbot_Interface SHALL enable vertical scrolling
3. WHEN a new message is added, THE Chatbot_Interface SHALL automatically scroll to the bottom
4. THE Chatbot_Interface SHALL maintain scroll position when Survey_Participants manually scroll upward
