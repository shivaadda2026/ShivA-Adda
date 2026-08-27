# Shiva Adda V2 - Thinking Agent Setup

### Isme kya naya hai?
V1 me sirf pehla reply jaata tha. V2 me:
- User aage kuch bhi puchega - "CIBIL kam hai", "Salary 15k hai", "Kaunsa best hai" - AI uski soch samajh ke reply dega
- Har user ki alag memory rakhega
- Stage by stage lead ko qualify karega

### Claude ke Connectors me kaise lagana hai?

1. Claude.ai > New Project > "Shiva Adda Agent"
2. Project Instructions me ye daal de:

```
Tu Shiva Adda ka Financial Advisor hai.
Har user ka message aayega, tujhe THINKING aur REPLY format me jawab dena hai.
User ki history bhi bhejunga.
Links: CARD={link}, LOAN={link}
```

3. Connectors > Add Tools > 
   - Tool 1: Instagram DM (Webhook URL)
   - Tool 2: Google Sheets (Leads save)
   - Tool 3: Banksathi API (optional)

4. Custom Tool Code:

```json
{
  "name": "send_banksathi_link",
  "description": "Send Banksathi link after qualification",
  "parameters": {
    "user_id": "string",
    "product": "CARD|LOAN|ACCOUNT",
    "qualified": "boolean"
  }
}
```

### Free me bina Claude ke chalana hai?

app_v2.py me _rule_based_thinking_reply already hai. Bina API key ke bhi kaam karega.

Test karne ke liye:

```bash
pip install Flask python-dotenv requests
python app_v2.py
```

Fir Postman se:

```
POST http://localhost:5000/chat
{
  "user_id": "123",
  "message": "Mera CIBIL 650 hai card milega?"
}
```

Reply ayega:
> "650 pe normal wala thoda mushkil hai bhai, par tension nahi. FD wala card 100% mil jayega..."

### Aage ka chat flow kaise sochega?

Example:
User: CARD chahiye
Bot: Age aur CIBIL kitna hai?
User: 22 hai, CIBIL 650
Bot: [Thinking] CIBIL kam hai, secured card dena chahiye
Bot Reply: FD wala card lele...

Yehi teri "their thinking" wali demand hai.

Deploy same Render pe hoga. Bas app_v2.py ko start karna hai.

.env me CLAUDE_API_KEY daal dega toh aur tez dimag se kaam karega, nahi daalega toh bhi rule based chal jayega.
