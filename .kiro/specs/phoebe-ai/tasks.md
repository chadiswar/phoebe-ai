# Implementation Plan: AP Chatbot Survey System

## Overview

This implementation plan breaks down the construction of a three-chatbot survey system into discrete coding tasks. The system consists of three standalone HTML pages with embedded CSS and JavaScript, each presenting a chatbot with a distinct personality (sarcastic, friendly, casual teen). The implementation follows a progressive approach: establish the base structure, implement core functionality, add personality variations, and validate with tests.

## Tasks

- [ ] 1. Create base HTML structure and design system
  - Create index.html with semantic HTML structure (header, chat container, input section)
  - Implement CSS design system with purple gradient theme (#667eea to #764ba2)
  - Add system font stack and typography styles
  - Implement responsive layout with 700px max-width container
  - Add CSS animations (fadeIn keyframes with translateY)
  - _Requirements: 6.1, 6.2, 8.1, 8.2_

- [ ]* 1.1 Write property test for visual consistency across pages
  - **Property 7: Visual Consistency Across Pages**
  - **Validates: Requirements 6.1, 6.2, 6.3**

- [ ] 2. Implement chat container and message rendering system
  - [ ] 2.1 Create chat container with 400px height and auto-scroll
    - Add scrollable div with overflow-y: auto
    - Implement message DOM structure (label + content)
    - _Requirements: 15.1, 15.2_
  
  - [ ] 2.2 Implement addMessage function for dynamic message creation
    - Create message elements with appropriate CSS classes
    - Differentiate user messages (right-aligned, purple gradient) from AI messages (left-aligned, white)
    - Apply fadeIn animation to new messages
    - Implement auto-scroll behavior (scrollTop = scrollHeight)
    - _Requirements: 6.3, 6.4, 7.4, 7.5_
  
  - [ ]* 2.3 Write property tests for message rendering
    - **Property 5: Message Animation Application**
    - **Validates: Requirements 6.5, 7.5**
    - **Property 8: Message Type Differentiation**
    - **Validates: Requirements 6.4**
    - **Property 10: Auto-Scroll Behavior**
    - **Validates: Requirements 7.4, 15.3**
    - **Property 11: Message Bubble Width Constraint**
    - **Validates: Requirements 8.3**

- [ ] 3. Implement question button grid and interaction system
  - [ ] 3.1 Create question button grid with CSS Grid layout
    - Add four question buttons with data-key attributes
    - Implement hover effects (border color change, upward translation)
    - Add active state styling and pointer cursor
    - _Requirements: 2.1, 2.3, 14.1, 14.2, 14.3, 14.4, 14.5_
  
  - [ ] 3.2 Implement button click handler and hiding logic
    - Create sendMessage function triggered by onclick
    - Hide clicked button using display: none
    - Maintain visibility of unclicked buttons
    - _Requirements: 3.1, 3.2_
  
  - [ ]* 3.3 Write property tests for question button behavior
    - **Property 1: Question Set Consistency**
    - **Validates: Requirements 2.1, 2.2, 2.4**
    - **Property 2: Question Button State Management**
    - **Validates: Requirements 3.1, 3.2**
    - **Property 16: Button Hover Effects**
    - **Validates: Requirements 14.1, 14.2, 14.3**
    - **Property 17: Button Interaction Styling**
    - **Validates: Requirements 14.4, 14.5**

- [ ] 4. Checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 5. Implement state management and response system
  - [ ] 5.1 Create state tracking with Set data structure
    - Initialize askedQuestions Set and totalQuestions constant
    - Add questions to Set when buttons are clicked
    - Track question state per session (no persistence)
    - _Requirements: 3.3, 3.4_
  
  - [ ] 5.2 Create response library for Sarcastic Bot
    - Define responses object with four question keys (uppercase)
    - Add two response strings per question with sarcastic personality
    - Add two greeting messages with condescending tone
    - Add two farewell messages
    - _Requirements: 4.1, 4.4, 13.1, 13.2_
  
  - [ ] 5.3 Implement response delivery with timing delays
    - Display user message immediately when button clicked
    - Delay first AI response by 500ms
    - Delay subsequent AI responses by 800ms each
    - Use nested setTimeout for sequential execution
    - _Requirements: 7.1, 7.2, 7.3, 4.5_
  
  - [ ]* 5.4 Write property tests for state and response system
    - **Property 3: Session State Isolation**
    - **Validates: Requirements 3.3**
    - **Property 4: Response Structure Consistency**
    - **Validates: Requirements 4.4**
    - **Property 9: Response Timing Sequence**
    - **Validates: Requirements 7.2, 7.3, 4.5**
    - **Property 20: Question Tracking Accuracy**
    - **Validates: Requirements 3.4**

- [ ] 6. Implement completion detection and navigation
  - [ ] 6.1 Create checkIfComplete function
    - Check if askedQuestions.size equals totalQuestions
    - Display farewell messages with 1000ms delays
    - Hide question selection interface
    - Show navigation button after completion
    - _Requirements: 5.1, 5.2, 10.1_
  
  - [ ] 6.2 Add navigation button for Sarcastic Bot
    - Create "Continue to Next Chatbot" button (initially hidden)
    - Set onclick to navigate to chatbot-friendly.html using window.location.href
    - _Requirements: 1.2, 10.2, 10.5_
  
  - [ ]* 6.3 Write property tests for completion detection
    - **Property 6: Completion Detection**
    - **Validates: Requirements 5.1, 5.2**
    - **Property 13: Navigation Button Visibility**
    - **Validates: Requirements 10.1**
    - **Property 14: Direct File Navigation**
    - **Validates: Requirements 10.5**

- [ ] 7. Implement initial greeting system
  - [ ] 7.1 Add greeting messages on page load
    - Display two AI greeting messages before user interaction
    - Use personality-appropriate tone for each chatbot
    - _Requirements: 13.1, 13.5_
  
  - [ ]* 7.2 Write property test for initial greetings
    - **Property 15: Initial Greeting Messages**
    - **Validates: Requirements 13.1, 13.5**

- [ ] 8. Checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 9. Create Friendly Bot (chatbot-friendly.html)
  - [ ] 9.1 Duplicate index.html structure and styling
    - Copy complete HTML structure, CSS, and JavaScript
    - Update header to "Friendly Bot" and "Friendly Mode"
    - _Requirements: 1.3, 6.1, 6.2_
  
  - [ ] 9.2 Create Friendly Bot response library
    - Replace response library with enthusiastic, supportive tone
    - Add two greeting messages with welcoming tone
    - Add two farewell messages
    - _Requirements: 4.2, 4.4, 13.3_
  
  - [ ] 9.3 Update navigation to Teen Bot
    - Set navigation button to navigate to chatbot-teen.html
    - _Requirements: 1.3, 10.3_
  
  - [ ]* 9.4 Write unit tests for Friendly Bot navigation
    - Test navigation button links to chatbot-teen.html
    - Test farewell messages have friendly tone
    - _Requirements: 1.3, 10.3_

- [ ] 10. Create Teen Bot (chatbot-teen.html)
  - [ ] 10.1 Duplicate chatbot-friendly.html structure
    - Copy complete HTML structure, CSS, and JavaScript
    - Update header to "Teen Bot" and "Chill Mode"
    - _Requirements: 1.4, 6.1, 6.2_
  
  - [ ] 10.2 Create Teen Bot response library with lowercase text
    - Replace response library with casual, laid-back tone
    - Ensure all response text is lowercase
    - Add two greeting messages with casual tone (lowercase)
    - Add two farewell messages (lowercase)
    - _Requirements: 4.3, 4.4, 13.4_
  
  - [ ] 10.3 Apply lowercase CSS styling
    - Add text-transform: lowercase to headers, labels, and messages
    - Apply to all text elements for consistent casual appearance
    - _Requirements: 11.1, 11.2, 11.3, 11.4_
  
  - [ ] 10.4 Replace navigation with survey instructions
    - Remove navigation button
    - Add final message box with survey instructions
    - Include "see Phoebe for a survey" text
    - Apply purple accent styling to final message
    - _Requirements: 1.5, 5.3, 5.4, 10.4, 12.1, 12.2, 12.3, 12.4_
  
  - [ ]* 10.5 Write unit tests for Teen Bot specific features
    - Test Teen Bot has no navigation button
    - Test final message contains "Phoebe" and "survey"
    - Test final message has purple accent styling
    - Test Teen Bot applies lowercase CSS transform
    - _Requirements: 1.4, 1.5, 11.1, 11.2, 12.1, 12.2, 12.3, 12.4_
  
  - [ ]* 10.6 Write property test for Teen Bot lowercase responses
    - **Property 19: Teen Bot Lowercase Responses**
    - **Validates: Requirements 4.3**

- [ ] 11. Checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 12. Implement responsive design refinements
  - [ ] 12.1 Add mobile viewport optimizations
    - Test layout at 320px minimum width
    - Ensure touch-friendly button sizes
    - Verify message bubbles scale properly
    - _Requirements: 8.2, 8.3, 8.4_
  
  - [ ]* 12.2 Write unit tests for responsive behavior
    - Test container adapts to viewport width
    - Test minimum 320px width support
    - Test message bubble max-width constraint
    - _Requirements: 8.1, 8.2, 8.3_

- [ ] 13. Validate static deployment requirements
  - [ ] 13.1 Verify client-side only implementation
    - Confirm no backend server dependencies
    - Confirm no data persistence mechanisms
    - Verify all functionality works with file:// protocol
    - _Requirements: 9.1, 9.2, 9.3, 9.4_
  
  - [ ]* 13.2 Write property test for client-side implementation
    - **Property 12: Client-Side Only Implementation**
    - **Validates: Requirements 9.2, 9.4**
  
  - [ ]* 13.3 Write unit test for static deployment
    - Test system functions without server infrastructure
    - Test no network requests in JavaScript code
    - _Requirements: 9.1, 9.3_

- [ ] 14. Verify chat container scrolling behavior
  - [ ]* 14.1 Write property test for chat container constraints
    - **Property 18: Chat Container Constraints**
    - **Validates: Requirements 15.1, 15.2**
  
  - [ ]* 14.2 Write unit tests for scroll behavior
    - Test auto-scroll on new message
    - Test manual scroll position preservation
    - _Requirements: 15.3, 15.4_

- [ ] 15. Final checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Each task references specific requirements for traceability
- The implementation uses HTML, CSS, and JavaScript as specified in the design document
- All three chatbot pages share identical structure with personality-specific content variations
- Property tests validate universal correctness properties across all scenarios
- Unit tests validate specific examples, edge cases, and configuration details
- Checkpoints ensure incremental validation throughout the implementation process
- No backend infrastructure or data persistence is required (static deployment)
