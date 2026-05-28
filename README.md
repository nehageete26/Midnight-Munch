# Midnight-Munch

<img width="629" height="177" alt="image" src="https://github.com/user-attachments/assets/6cd40888-1671-47bd-aafb-b7447a340adb" />



## 📌 Overview
Midnight Munch is a fully automated Telegram food ordering bot built with n8n (no code / low code workflow automation). Users can chat naturally with the bot to browse the menu, check inventory, place orders, and get answers to FAQs — all without any manual intervention. Every order is automatically logged into a Google Sheet.


## 🔄 How it works
<img width="687" height="206" alt="image" src="https://github.com/user-attachments/assets/5ba17eb2-5401-4b0e-b696-0c9d25392f8a" />


## ✨ Features
📋 Menu lookup — lists all available dishes with real-time stock count
🛒 Order placement — collects name + quantity, confirms order instantly
📦 Inventory check — reads live data from Google Sheets (marks out-of-stock items)
❓ FAQ answering — opening hours, delivery range, payment methods, refund policy
🚫 Off-menu handling — politely declines unavailable items (e.g. pizza) and redirects
💾 Auto order logging — every confirmed order is appended to a Google Sheet with timestamp
🧠 Conversation memory — uses Simple Memory node so context is retained mid-conversation


## 🛠️ Tech stack
n8n —> workflow automation platform (self-hosted / cloud)
Telegram Bot API —> messaging interface via BotFather
Google Gemini (via OpenRouter) —> AI language model powering the agent
Google Sheets —> three-tab database: inventory, questions (FAQs), orders
Simple Memory node —> maintains session-level conversation context


## 📊 Google Sheets structure
A single spreadsheet with 3 tabs acts as the entire backend:
📄 inventory — Food Item / Quantity / Status
📄 questions — Question / Answer (FAQs)
📄 orders — Customer Name / Food Item / Qty / Order Date / Status


## 🧩 n8n workflow nodes
Telegram Trigger —> listens for incoming messages
AI Agent —> core reasoning node with tools attached
OpenRouter Chat Model —> powers the AI agent (Google Gemini)
Simple Memory —> stores conversation context per session
FAQs (Google Sheets read) —> tool for answering common questions
get inventory (Google Sheets read) —> tool to check stock availability
orders by customer data add (Google Sheets append) —> tool to log new orders
Send a text message (Telegram) —> sends the AI response back to user on telegram


## 🚀 Setup guide
Create a Telegram bot via @BotFather and copy the API token
Set up a Google Sheet with the 3 tabs (inventory, questions, orders) as shown above
In n8n, import the workflow JSON and connect your credentials:
Telegram API token
Google Sheets OAuth2
OpenRouter API key (for Gemini access)
Paste your Google Sheet ID in each Sheets node
Activate the workflow — the bot is live!


## 💡 System prompt (AI Agent)
You are a smart food ordering assistant for "Midnight Munch Restaurant".  
Rules:  
1. When a user sends their first message (like hi, hello), reply with:  
"Welcome to Midnight Munch Restaurant 🍽️  
How can I help you today?  
- 🛒 Place an order  
- ℹ️ FAQ / Information  
- 📦 Check order / stock"  
(👉 Keep the tone friendly like a food delivery app, not like a product e-commerce site)  
2. If user wants *order*:  
- Ask step by step: name, food item, quantity  
- Check Inventory sheet before confirming  
   - If available → "Your order for *[item]* (x quantity) is confirmed ✅"  
   - If not available → "Sorry, *[item]* is out of stock ❌. Available options: [list items]"  
- Only confirmed orders go into the Orders sheet  
- In Orders sheet always include:  
   Status = Confirmed / Rejected  
   Description = "Order accepted (item available)" OR "Order rejected (out of stock)"  
3. If user wants *FAQ*:  
- Answer short and clear (delivery time, payment method, restaurant hours)  
4. If user wants *check order* or *check stock*:  
- Ask for food name or order id  
- Look up in Google Sheets (Orders / Inventory)  
   - If checking order → reply with status (Confirmed / Rejected / Cancelled / Delivered)  
   - If checking stock → reply with available quantity of that food  
   - Also list all *available* food items with quantity if requested  
5. If user wants *cancel order*:  
- Reply politely:  
  "Sorry 🙏 I cannot cancel orders directly.  
   Please call the restaurant owner first and inform them.  
   Owner Contact: +95 1224567890"  
6. Always reply in short text like a normal WhatsApp chat  
- Do not use **bold** or long paragraphs  
- Use *stars* only for highlighting words  
- Keep tone friendly, polite, and food-delivery app style



## 🤝 Contributing
Pull requests are welcome! Ideas for improvements: order cancellation flow, payment integration, multi-language support, WhatsApp channel support. 
