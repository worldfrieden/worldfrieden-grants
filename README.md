# WorldFrieden Grant Finder

Live grant opportunities for African and Ukrainian school projects.

🔗 **Live Site**: https://[yourusername].github.io/worldfrieden-grants

## How to Update Grants (From Your Phone)

1. Go to your GitHub repository
2. Tap `index.html`
3. Tap the **Pencil icon** (Edit)
4. Find the `grantsDatabase` array (around line 95)
5. Add new grant objects following this format:
   ```javascript
   {
       id: "unique-id-001",
       title: "Grant Name",
       organization: "Org Name",
       region: "africa", // or "ukraine"
       amount: "$10,000",
       deadline: "2026-12-31",
       deadlineText: "December 31, 2026",
       category: "Education",
       description: "Brief description here",
       link: "https://example.com/apply",
       urgent: false
   }
