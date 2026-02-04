LIVE DEMO 
🔗
https://sruthi007916.github.io/AI-Chatbot/



# AI-Chatbot
Founder Friendly AI - a basic frontend prototype using HTML, CSS and JavaScript

HTML


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI Chatbot</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <div class="chat-container">
    <div class="chat-header">
      <h1>AI Chatbot</h1>
    </div>

    <div class="chat-messages" id="chatMessages">
      <div class="message bot-message">
        <p>Hello! I'm your AI Assistant. How can I help you today?</p>
      </div>
    </div>

    <div class="chat-input">
      <input
        type="text"
        id="user-input"
        placeholder="Type your message here..."
      />
      <button id="send-button">Send</button>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
                                                                                                             
                                                                                                             
CSS


* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, sans-serif;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f0f2f5;
}

.chat-container {
  width: 400px;
  height: 600px;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.chat-header {
  padding: 20px;
  background-color: #075e54;
  color: white;
  border-radius: 10px 10px 0 0;
}

.chat-header h1 {
  font-size: 1.5rem;
  text-align: center;
}

.chat-messages {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

.message {
  margin-bottom: 15px;
  max-width: 80%;
  padding: 10px 15px;
  border-radius: 15px;
  word-wrap: break-word;
}

.bot-message {
  background-color: #e9ecef;
  align-self: flex-start;
  border-radius: 15px 15px 15px 0;
}

.user-message {
  background-color: #075e54;
  color: white;
  margin-left: auto;
  border-radius: 15px 15px 0 15px;
}

.chat-input {
  padding: 20px;
  display: flex;
  gap: 10px;
  border-top: 1px solid #e9ecef;
}

#user-input {
  flex: 1;
  padding: 10px;
  border: 1px solid #e9ecef;
  border-radius: 5px;
  outline: none;
}

#send-button {
  padding: 10px 20px;
  background-color: #075e54;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

#send-button:hover {
  background-color: #128c7e;
}


Javascript 

document.addEventListener("DOMContentLoaded", () => {

  const chatMessages = document.getElementById("chatMessages");
  const userInput = document.getElementById("user-input");
  const sendButton = document.getElementById("send-button");

  // ---------- RULE-BASED RESPONSES ----------
  const botResponses = [
    {
      keywords: ["hello", "hi", "hey"],
      response: "Hello! 👋 I am Founder Friendly AI. How can I help you today?"
    },
    {
      keywords: ["your name", "what is your name", "who are you"],
      response: "My name is Founder Friendly AI 🤖. I validate startup ideas."
    },
    {
      keywords: ["how are you"],
      response: "I'm doing great! 😊 How about you?"
    },
    {
      keywords: ["i am fine", "i'm fine", "good", "fine"],
      response: "That's great to hear! 😊 Tell me about your startup idea."
    },
    {
      keywords: ["thank you", "thanks"],
      response: "You're welcome! 😊"
    },
    {
      keywords: ["bye", "goodbye"],
      response: "Goodbye! 👋 Best of luck with your startup journey!"
    }
  ];

  // ---------- AI AGENT STATE ----------
  let agentState = "idle";
  let ideaData = {};

  function addMessage(message, isUser = false) {
    const messageDiv = document.createElement("div");
    messageDiv.classList.add("message", isUser ? "user-message" : "bot-message");

    const messageText = document.createElement("p");
    messageText.textContent = message;

    messageDiv.appendChild(messageText);
    chatMessages.appendChild(messageDiv);
    chatMessages.scrollTop = chatMessages.scrollHeight;
  }

  function getBotResponse(userMessage) {
    const msg = userMessage.toLowerCase();

    // ---------- RESET ----------
    if (
      msg.includes("next") ||
      msg.includes("new idea") ||
      msg.includes("another idea") ||
      msg.includes("restart")
    ) {
      agentState = "idle";
      ideaData = {};
      return "🔄 Sure! Let's start fresh. What is your startup idea?";
    }

    // ---------- RULE-BASED ----------
    if (agentState === "idle") {
      for (const item of botResponses) {
        for (const keyword of item.keywords) {
          if (msg.includes(keyword)) {
            return item.response;
          }
        }
      }
    }

    // ---------- AI AGENT FLOW ----------
    if (agentState === "idle") {
      agentState = "ask_problem";
      return "Great 🚀 What problem does your idea solve?";
    }

    if (agentState === "ask_problem") {
      ideaData.problem = userMessage;
      agentState = "ask_users";
      return "Who are the target users for this problem?";
    }

    if (agentState === "ask_users") {
      ideaData.users = userMessage;
      agentState = "ask_revenue";
      return "How will you earn money from this idea?";
    }

    // ---------- FINAL + AI SUGGESTION ----------
    if (agentState === "ask_revenue") {
      ideaData.revenue = userMessage;
      agentState = "final";

      let domain = "general";
      const problemText = ideaData.problem.toLowerCase();

      if (problemText.includes("food") || problemText.includes("health")) {
        domain = "food";
      } else if (problemText.includes("app") || problemText.includes("software")) {
        domain = "tech";
      } else if (problemText.includes("education") || problemText.includes("student")) {
        domain = "education";
      } else if (problemText.includes("service") || problemText.includes("delivery")) {
        domain = "service";
      }

      let suggestions = "";
      const revenueText = msg;

      if (
        revenueText.includes("dont know") ||
        revenueText.includes("don't know") ||
        revenueText.includes("no idea")
      ) {
        if (domain === "food") {
          suggestions = `
💡 AI Suggested Revenue Models:
• Product sales (online & stores)
• Monthly nutrition packs
• School partnerships
`;
        } else if (domain === "tech") {
          suggestions = `
💡 AI Suggested Revenue Models:
• Subscription
• Freemium + premium
• Licensing
`;
        } else if (domain === "education") {
          suggestions = `
💡 AI Suggested Revenue Models:
• Course fees
• Certifications
• Institutional tie-ups
`;
        } else {
          suggestions = `
💡 AI Suggested Revenue Models:
• Direct sales
• Subscription
• Advertising
`;
        }
      }

      return `✅ Startup Idea Validation Result

🔹 Problem:
${ideaData.problem}

🔹 Target Users:
${ideaData.users}

🔹 Revenue Model:
${ideaData.revenue}

${suggestions}

🧠 AI Insight:
Your idea belongs to the **${domain.toUpperCase()} domain**.
It has real-world potential if executed properly.

🚀 Suggested Next Steps:
• Build a simple MVP
• Test with users
• Improve and scale`;
    }

    return "🤔 Please tell me about your startup idea.";
  }

  function sendMessage() {
    const message = userInput.value.trim();
    if (!message) return;

    addMessage(message, true);
    userInput.value = "";

    setTimeout(() => {
      const botReply = getBotResponse(message);
      addMessage(botReply);
    }, 300);
  }

  sendButton.addEventListener("click", sendMessage);
  userInput.addEventListener("keydown", (e) => {
    if (e.key === "Enter") sendMessage();
  });

});
