# BookishBot-ChatBot
A full-stack customer support chatbot

## Functionality:
1. User can request book recommendations.
2. Chat recommends books and links to relevant website sections.
3. Spam protection with rate limiting on API endpoints.
   
### Additional features:
1. Ability to search for specific book titles.
2. Chat recommendations based on book content knowledge.

## Features
 1. Real-time streamed chatbot responses
 2. Optimistic updates for the best UX
 3. Secured, rate-limited API routes to prevent abuse
 4. Built with TypeScript
 5. TailwindCSS
 6. Icons from Lucide
 7. Class merging with tailwind-merge
 8. Conditional classes with clsx

 ---

 <details>
   <summary>Project setup </summary>
   Summary:

1. **Project Objective**: Develop a Next.js chatbot focused on assisting bookstore customers, ensuring a streamlined user experience by rejecting non-bookstore-related queries.

2. **Setup**: Initialized project with Next.js v13, TypeScript, and Tailwind CSS for styling.use ```npx create-next-app```

3. **Routing**: Utilized Next.js file-based routing structure for managing URLs and components.

4. **Mocking Content**: Placeholder website content added to `index.tsx`.

5. **Layout**: Implemented a main layout (`Layout.tsx`) for consistent page structure.

6. **Documentation**: Notes and instructions recorded in a Markdown file for reference.

This setup establishes the foundation for building the specialized chatbot application tailored to bookstore inquiries.
 </details>

---
<details>
   <summary> Creating Collapse App Section
   </summary>

   Sure, here's a more detailed explanation with code snippets:

1. **Chat Component Integration**:
   
   In the project, a chat component is added to the root layout component (`Layout.tsx`) to ensure it appears consistently across all pages. This ensures that users have easy access to the chat functionality regardless of the page they're on.

   ```tsx
   // Layout.tsx
   
   import Chat from './components/Chat';
   
   const Layout: React.FC = ({ children }) => {
       return (
           <div className="layout">
               <Chat /> {/* Including the chat component */}
               <main>{children}</main>
           </div>
       );
   };
   ```

2. **Metadata Update**:
   
   The website's metadata is updated to reflect its bookstore-focused content. This includes setting the title and description appropriately.

   ```html
   <!-- _document.tsx or equivalent -->
   
   <Head>
       <title>Book Buddy</title>
       <meta name="description" content="Your bookstore for fantasy and mystery novels." />
       {/* Other metadata */}
   </Head>
   ```

3. **UI Library Integration**:
   
   The Radix UI library is utilized to incorporate an accessible accordion component for managing chat expansion and collapse. The accordion component provides a seamless user experience and handles accessibility concerns automatically.

   ```bash
   # Install Radix UI library
   npm install @radix-ui/react-accordion
   ```

4. **Accordion Styling**:
   
   Styling is applied to the accordion component to ensure it aligns with the overall design of the chat interface. This involves setting specific classes for size, positioning, and visual styling.

   ```tsx
   // Chat.tsx
   
   import { Accordion, AccordionItem, AccordionTrigger, AccordionContent } from '@radix-ui/react-accordion';
   
   const Chat: React.FC = () => {
       return (
           <Accordion>
               <AccordionItem value="item-1">
                   <AccordionTrigger className="accordion-trigger">Chat</AccordionTrigger>
                   <AccordionContent className="accordion-content">
                       {/* Chat content goes here */}
                   </AccordionContent>
               </AccordionItem>
           </Accordion>
       );
   };
   ```
   1. Accordion component is used to manage the collapsible behavior of the chat interface.
   2. The AccordionTrigger component serves as the clickable element that expands and collapses the chat interface. It includes the text "Chat" and an icon (such as a Chevron) for visual indication of the expansion state.
   3. The AccordionContent component contains the actual content of the chat interface, such as chat messages and user input. This content is initially hidden and revealed upon expanding the accordion.

5.**// libs/util/utils.ts**
 Let's include the setup for the libs/util directory, which includes the utils.ts file for merging class names:

import clsx from 'clsx';
import twMerge from 'tailwind-merge';

export const mergeClassNames = (...classNames: (string | undefined | null | false)[]): string => {
    return clsx(classNames.filter(Boolean));
};

Including the libs/util/utils.ts file ensures that the utility function for merging class names is available for use within the chat component (Chat.tsx). This function helps streamline class name management, contributing to cleaner and more maintainable code.

6. **Chat Header Implementation**:
   
   A chat header component is created to display relevant information at the top of the chat interface. This may include indicators for online status and the type of support provided.it shows in without expanded condition as defualt.

   ```tsx
   // ChatHeader.tsx
   
   const ChatHeader: React.FC = () => {
       return (
           <div className="chat-header">
               <p>Chat with</p>
               <div className="online-status"></div> {/* Online status indicator */}
               <p className="support-type">Book Support</p> {/* Support type */}
           </div>
       );
   };
   ```

7. **Accordion Trigger Customization**:
   
   The accordion trigger component is customized to include visual feedback, such as a Chevron icon, indicating the expansion and collapse state of the chat interface.

   ```tsx
   // Chat.tsx
   
   <AccordionTrigger className="accordion-trigger">
       Chat
       <ChevronIcon />
   </AccordionTrigger>
   ```

8. **Accordion Content Setup**:
   
   The accordion content structure is defined to include placeholders for displaying chat messages and user input. This ensures that the chat interface is properly organized and ready to display dynamic content.

   ```tsx
   // Chat.tsx
   
   <AccordionContent className="accordion-content">
       <div className="chat-messages">
           {/* Placeholder for chat messages */}
       </div>
       <div className="chat-input">
           {/* Placeholder for chat input */}
       </div>
   </AccordionContent>
   ```


These steps provide a comprehensive overview of integrating the chat component, setting up metadata, utilizing a UI library, customizing components, and preparing the chat interface with necessary content placeholders and user interaction elements. Each component plays a crucial role in creating a functional and user-friendly chat interface tailored for bookstore-related inquiries.
</details>

---
<details>
   <summary>Creating the Chat Input Component</summary>


### Creating the Chat Input Component

The chat input component will be responsible for allowing users to input messages in the chat interface. This component will demonstrate core concepts of modern web development including React Context for sharing state between components and React Query for data fetching.

#### Step 1: Define Props Interface (Typescript Specific)

```typescript
// ChatInput.tsx

interface ChatInputProps extends React.HTMLAttributes<HTMLDivElement> {
  // Custom props can be added here
}
```

#### Step 2: Implement the Chat Input Component

```tsx
// ChatInput.tsx

import React, { useState } from 'react';
import { TextAreaAutoSize } from 'react-textarea-autosize'; // Assuming you've installed this package

const ChatInput: React.FC<ChatInputProps> = ({ className, ...props }) => {
  const [input, setInput] = useState('');

  const handleKeyDown = (e: React.KeyboardEvent<HTMLTextAreaElement>) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      sendMessage();
    }
  };

  const sendMessage = async () => {
    // Logic to send message to API endpoint
    // Example: Use fetch or Axios to send message
    try {
      const response = await fetch('/api/message', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ text: input }),
      });
      if (response.ok) {
        // Handle success
      } else {
        // Handle error
      }
    } catch (error) {
      // Handle error
    }
  };

  return (
    <div className={`relative mt-4 flex-1 overflow-hidden rounded-lg border-none outline-none ${className}`} {...props}>
      <TextAreaAutoSize
        rows={2}
        maxRows={4}
        value={input}
        onChange={(e) => setInput(e.target.value)}
        onKeyDown={handleKeyDown}
        placeholder="Write a message..."
        className="w-full p-4 resize-none bg-gray-100 text-gray-900 focus:ring-0 focus:text-sm focus:leading-6"
        autoFocus
      />
    </div>
  );
};

export default ChatInput;
```

#### Step 3: Implement API Endpoint (Server-side)

In your Next.js API route (usually located in `pages/api` directory), implement the `/api/message` endpoint to handle incoming messages.

```javascript
// pages/api/message.js

export default function handler(req, res) {
  if (req.method === 'POST') {
    // Handle incoming message
    const { text } = req.body;

    // Example: Process the message and return a response
    const responseMessage = `Received message: ${text}`;
    
    // Return a response (in this case, echoing back the received message)
    res.status(200).json({ message: responseMessage });
  } else {
    res.status(405).json({ error: 'Method Not Allowed' });
  }
}
```

#### Step 4: Implement React Query for Sending Messages

```tsx
// ChatInput.tsx

import { useMutation } from 'react-query'; // Assuming you've installed this package

const ChatInput: React.FC<ChatInputProps> = ({ className, ...props }) => {
  const [input, setInput] = useState('');
  const sendMessageMutation = useMutation(sendMessage);

  const handleKeyDown = (e: React.KeyboardEvent<HTMLTextAreaElement>) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      sendMessageMutation.mutate(input);
    }
  };

  const sendMessage = async (message: string) => {
    try {
      const response = await fetch('/api/message', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ text: message }),
      });
      if (response.ok) {
        // Handle success
      } else {
        // Handle error
      }
    } catch (error) {
      // Handle error
    }
  };

  return (
    // JSX code remains the same
  );
};
```

### Explanation:

1. **Props Interface**: Defines the props interface `ChatInputProps` which extends `HTMLAttributes<HTMLDivElement>` to allow passing custom class names and other HTML attributes.

2. **Chat Input Component**: Implements the `ChatInput` component which consists of a resizable text area for typing messages. It handles sending messages on pressing the Enter key.

3. **API Endpoint**: Implements the server-side API endpoint (`/api/message`) using Next.js API routes to handle incoming messages. It receives the message from the client and returns a response.

4. **React Query for Sending Messages**: Integrates React Query's `useMutation` hook to handle sending messages asynchronously. It provides a cleaner way to manage loading states and error handling for the message sending process.

These steps demonstrate the implementation of the chat input component along with necessary backend logic to handle incoming messages and send responses.
</details>

---
<details>
   <summary>Setting up API Routes</summary>

![image](https://github.com/Akmeena4u/BookishBot-ChatBot/assets/93425334/b3636067-b200-4bbf-a8f3-4b80a69f202a)

</details>

 
