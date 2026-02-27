<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WorldFrieden Grant Finder</title>
    <meta name="description" content="Current grants for African and Ukrainian school projects">
    <meta name="theme-color" content="#2563eb">
    <link rel="manifest" href="manifest.json">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; background: #f3f4f6; color: #1f2937; line-height: 1.6; padding-bottom: 80px; }
        header { background: linear-gradient(135deg, #1e40af 0%, #3b82f6 100%); color: white; padding: 1.5rem 1rem; position: sticky; top: 0; z-index: 100; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        h1 { font-size: 1.25rem; margin-bottom: 0.5rem; display: flex; align-items: center; gap: 0.5rem; }
        .subtitle { font-size: 0.875rem; opacity: 0.9; }
        .controls { background: white; padding: 1rem; position: sticky; top: 80px; z-index: 99; border-bottom: 1px solid #e5e7eb; display: flex; flex-direction: column; gap: 0.75rem; }
        .filter-group { display: flex; gap: 0.5rem; overflow-x: auto; padding-bottom: 0.25rem; scrollbar-width: none; }
        .filter-group::-webkit-scrollbar { display: none; }
        button { padding: 0.5rem 1rem; border: 1px solid #d1d5db; background: white; border-radius: 2rem; font-size: 0.875rem; cursor: pointer; white-space: nowrap; transition: all 0.2s; }
        button.active { background: #2563eb; color: white; border-color: #2563eb; }
        button.saved-filter { background: #dcfce7; border-color: #16a34a; color: #166534; }
        input[type="search"] { width: 100%; padding: 0.75rem 1rem; border: 1px solid #d1d5db; border-radius: 0.5rem; font-size: 1rem; }
        .stats { display: flex; justify-content: space-between; padding: 0.5rem 1rem; font-size: 0.75rem; color: #6b7280; background: #f9fafb; }
        .grant-grid { display: grid; gap: 1rem; padding: 1rem; max-width: 800px; margin: 0 auto; }
        .grant-card { background: white; border-radius: 0.75rem; padding: 1.25rem; box-shadow: 0 1px 3px rgba(0,0,0,0.1); border-left: 4px solid #e5e7eb; transition: transform 0.2s, box-shadow 0.2s; position: relative; }
        .grant-card:hover { transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.15); }
        .grant-card.africa { border-left-color: #16a34a; }
        .grant-card.ukraine { border-left-color: #2563eb; }
        .grant-card.urgent { border-left-color: #dc2626; animation: pulse 2s infinite; }
        @keyframes pulse { 0%, 100% { box-shadow: 0 0 0 0 rgba(220, 38, 38, 0.2); } 50% { box-shadow: 0 0 0 4px rgba(220, 38, 38, 0.2); } }
        .card-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 0.75rem; gap: 0.5rem; }
        .region-badge { font-size: 0.625rem; font-weight: 700; text-transform: uppercase; padding: 0.25rem 0.5rem; border-radius: 0.25rem; letter-spacing: 0.05em; }
        .region-badge.africa { background: #dcfce7; color: #166534; }
        .region-badge.ukraine { background: #dbeafe; color: #1e40af; }
        .save-btn { background: none; border: none; font-size: 1.25rem; padding: 0.25rem; cursor: pointer; opacity: 0.5; transition: opacity 0.2s; }
        .save-btn:hover, .save-btn.saved { opacity: 1; }
        .save-btn.saved { color: #dc2626; }
        h2 { font-size: 1.125rem; margin-bottom: 0.25rem; color: #111827; line-height: 1.3; }
        .org { font-size: 0.875rem; color: #6b7280; margin-bottom: 0.75rem; }
        .details { display: grid; gap: 0.5rem; margin-bottom: 1rem; }
        .detail-row { display: flex; justify-content: space-between; font-size: 0.875rem; }
        .detail-label { color: #6b7280; }
        .detail-value { font-weight: 600; color: #111827; text-align: right; }
        .amount { color: #059669; font-size: 1rem; }
        .deadline { color: #dc2626; }
        .deadline.safe { color: #059669; }
        .description { font-size: 0.875rem; color: #4b5563; margin-bottom: 1rem; line-height: 1.5; display: -webkit-box; -webkit-line-clamp: 3; -webkit-box-orient: vertical; overflow: hidden; }
        .card-actions { display: flex; gap: 0.5rem; }
        .btn-primary { flex: 1; background: #2563eb; color: white; border: none; padding: 0.75rem; border-radius: 0.5rem; font-weight: 600; text-decoration: none; display: inline-block; text-align: center; }
        .btn-secondary { flex: 1; background: #f3f4f6; color: #374151; border: 1px solid #d1d5db; padding: 0.75rem; border-radius: 0.5rem; font-weight: 600; text-decoration: none; display: inline-block; text-align: center; }
        .empty-state { text-align: center; padding: 3rem 1rem; color: #6b7280; grid-column: 1 / -1; }
        .empty-state-icon { font-size: 3rem; margin-bottom: 1rem; }
        .updated { position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); background: #10b981; color: white; padding: 0.75rem 1.5rem; border-radius: 2rem; box-shadow: 0 4px 12px rgba(0,0,0,0.2); display: none; z-index: 1000; }
        @media (min-width: 640px) { .grant-grid { grid-template-columns: repeat(2, 1fr); } }
    </style>
</head>
<body>
<header>
    <h1>🌍 WorldFrieden Grants</h1>
    <div class="subtitle">School projects in Africa & Ukraine</div>
</header>

<div class="controls">
    <input type="search" id="searchInput" placeholder="Search grants...">
    <div class="filter-group">
        <button class="filter-btn active" data-filter="all">All</button>
        <button class="filter-btn" data-filter="africa">🌍 Africa</button>
        <button class="filter-btn" data-filter="ukraine">💙 Ukraine</button>
        <button class="filter-btn saved-filter" data-filter="saved">❤️ Saved</button>
    </div>
</div>

<div class="stats">
    <span id="count">Loading...</span>
    <span>Updated: <span id="lastUpdated">Today</span></span>
</div>

<div id="grantContainer" class="grant-grid">
    <div class="loading" style="grid-column: 1/-1; text-align: center; padding: 2rem;">Loading grants...</div>
</div>

<div class="updated" id="notification">Saved!</div>

<script>
// GRANT DATABASE - Edit this section to add/update grants
const grantsDatabase = [
    {
        id: "africa-001",
        title: "Education Cannot Wait - Emergency Response",
        organization: "Education Cannot Wait (ECW)",
        region: "africa",
        amount: "Up to $50,000",
        deadline: "2026-03-15",
        deadlineText: "Rolling basis",
        category: "Emergency",
        description: "Rapid funding for schools facing immediate crisis or closure. Supports teacher salaries, temporary learning spaces, and essential supplies.",
        link: "https://www.educationcannotwait.org",
        urgent: true
    },
    {
        id: "africa-002",
        title: "USADF Community Grants - Education",
        organization: "U.S. African Development Foundation",
        region: "africa",
        amount: "$10,000 - $250,000",
        deadline: "2026-04-30",
        deadlineText: "April 30, 2026",
        category: "Capacity Building",
        description: "Supports African-owned enterprises and community groups providing education and vocational training. 100% African-led implementation required.",
        link: "https://www.usadf.gov",
        urgent: false
    },
    {
        id: "ukraine-001",
        title: "UNICEF Ukraine Education in Emergencies",
        organization: "UNICEF / USAID",
        region: "ukraine",
        amount: "Up to $100,000",
        deadline: "2026-03-01",
        deadlineText: "March 1, 2026",
        category: "Emergency Response",
        description: "Immediate support for schools in bomb shelters and underground learning centers. Priority for frontline regions and displaced children.",
        link: "https://www.unicef.org/ukraine",
        urgent: true
    },
    {
        id: "ukraine-002",
        title: "Olena Zelenska Foundation - School Restoration",
        organization: "Olena Zelenska Foundation",
        region: "ukraine",
        amount: "Project-based",
        deadline: "2026-12-31",
        deadlineText: "Open all year",
        category: "Infrastructure",
        description: "Rebuilding destroyed schools and kindergartens. Partners with international donors for major construction projects.",
        link: "https://zelenskafoundation.org",
        urgent: false
    }
];

// App Logic
let currentFilter = 'all';
let savedGrants = JSON.parse(localStorage.getItem('savedGrants') || '[]');
const container = document.getElementById('grantContainer');
const searchInput = document.getElementById('searchInput');
const notification = document.getElementById('notification');

function renderGrants() {
    const searchTerm = searchInput.value.toLowerCase();
    let filtered = grantsDatabase;
    
    if (currentFilter === 'saved') {
        filtered = filtered.filter(g => savedGrants.includes(g.id));
    } else if (currentFilter !== 'all') {
        filtered = filtered.filter(g => g.region === currentFilter);
    }
    
    if (searchTerm) {
        filtered = filtered.filter(g => 
            g.title.toLowerCase().includes(searchTerm) ||
            g.organization.toLowerCase().includes(searchTerm) ||
            g.description.toLowerCase().includes(searchTerm)
        );
    }
    
    document.getElementById('count').textContent = `${filtered.length} grant${filtered.length !== 1 ? 's' : ''}`;
    
    if (filtered.length === 0) {
        container.innerHTML = `<div class="empty-state"><div class="empty-state-icon">🔍</div><h3>No grants found</h3><p>Try adjusting your search</p></div>`;
        return;
    }
    
    container.innerHTML = filtered.map(grant => {
        const isSaved = savedGrants.includes(grant.id);
        const daysLeft = Math.ceil((new Date(grant.deadline) - new Date()) / (1000 * 60 * 60 * 24));
        const deadlineClass = daysLeft < 7 ? 'deadline' : 'deadline safe';
        
        return `
            <article class="grant-card ${grant.region} ${grant.urgent ? 'urgent' : ''}" data-id="${grant.id}">
                <div class="card-header">
                    <span class="region-badge ${grant.region}">${grant.region === 'africa' ? '🌍 Africa' : '💙 Ukraine'}</span>
                    <button class="save-btn ${isSaved ? 'saved' : ''}" onclick="toggleSave('${grant.id}')">${isSaved ? '❤️' : '🤍'}</button>
                </div>
                <h2>${grant.title}</h2>
                <div class="org">${grant.organization}</div>
                <div class="details">
                    <div class="detail-row"><span class="detail-label">Amount:</span><span class="detail-value amount">${grant.amount}</span></div>
                    <div class="detail-row"><span class="detail-label">Deadline:</span><span class="detail-value ${deadlineClass}">${grant.deadlineText}</span></div>
                    <div class="detail-row"><span class="detail-label">Type:</span><span class="detail-value">${grant.category}</span></div>
                </div>
                <div class="description">${grant.description}</div>
                <div class="card-actions">
                    <a href="${grant.link}" target="_blank" class="btn-primary">Apply Now →</a>
                    <button class="btn-secondary" onclick="shareGrant('${grant.id}')">Share</button>
                </div>
            </article>
        `;
    }).join('');
}

function toggleSave(id) {
    const index = savedGrants.indexOf(id);
    if (index > -1) {
        savedGrants.splice(index, 1);
        showNotification('Removed');
    } else {
        savedGrants.push(id);
        showNotification('Saved!');
    }
    localStorage.setItem('savedGrants', JSON.stringify(savedGrants));
    renderGrants();
}

function shareGrant(id) {
    const grant = grantsDatabase.find(g => g.id === id);
    const text = `${grant.title} - ${grant.amount} - ${grant.link}`;
    if (navigator.share) {
        navigator.share({ title: grant.title, text: text, url: grant.link });
    } else {
        navigator.clipboard.writeText(text).then(() => showNotification('Copied!'));
    }
}

function showNotification(text) {
    notification.textContent = text;
    notification.style.display = 'block';
    setTimeout(() => notification.style.display = 'none', 2000);
}

// Event Listeners
document.querySelectorAll('.filter-btn').forEach(btn => {
    btn.addEventListener('click', (e) => {
        document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
        e.target.classList.add('active');
        currentFilter = e.target.dataset.filter;
        renderGrants();
    });
});

searchInput.addEventListener('input', () => renderGrants());
document.getElementById('lastUpdated').textContent = new Date().toLocaleDateString();

// Initial render
renderGrants();
</script>
</body>
</html>
