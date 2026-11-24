## Phase 5: Testing locally

To see this work with the `.env` values, you no longer double-click the file. You use the Vite server:

```bash
# In your terminal
npx vite
```
**The URL:** Open `http://localhost:5173`. 
Vite will automatically read the `.env`, replace the placeholder in your code, and the cart will talk to your backend on Port 8001 seamlessly.

### Why this is the "Apex" Way:
1.  **Identity Security:** Your API key is now hidden from your Git history. 
2.  **Zero Modification:** When you move from `localhost:8001` to `api.smartrecshub.com`, you only change the `.env` file. You don't have to touch the code.
3.  **Client Sample:** This entire repo now serves as a **"Client Implementation Template."** You can share the GitHub link with those 10 new clients so they can see exactly how to structure their integration.

**James, does this Vite-based environment setup feel manageable?** It is the standard way modern frontend developers protect keys. Shall we verify that your browser console now logs "Operational" without a hardcoded key?