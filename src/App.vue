<template>
  <div>
    <div>
      <input type="text" 
        v-model="search" 
        placeholder="Search by name, email, website, or company…"
        @keyup.enter="submitSearch(search)"
      />
      <button @click="submitSearch(search)">Search</button>
    </div>

    <div v-if="results.length === 0">
      No results found
    </div>

    <ul>
      <li v-for="list in results" :key="list.id" class="card">
        <div class="card__header">
          <div class="avatar">{{ (list.name).charAt(0).toUpperCase() }}</div>
          <div class="info">
            <h3 class="name" :title="list.name">{{ list.name }}</h3>
            <p class="email">{{ list.email }}</p>
          </div>
        </div>

        <div class="card__body">
          <a
            class="website"
            href="/"
            rel="noopener"
            :title="list.website"
          >
            🌐 {{ list.website }}
          </a>
          <p class="company">{{ list.company?.name }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      lists: [],
      results: [],
      search: ''
    }
  },
  methods: {
    async searchApi() {
      try {
        const url = 'https://jsonplaceholder.typicode.com/users'
        const res = await fetch(url)
        const data = await res.json()
        this.lists = data
        this.results = this.lists
      } catch (error) {
        console.log(error);
        
      }

    },
    submitSearch(search) {
      const searchWord = search.trim()
      if (searchWord === '') {
        this.results = this.lists
        return
      }
      this.results = this.lists.filter(list => {
        return (
          list.name.toLowerCase().includes(searchWord.toLowerCase()) ||
          list.email.toLowerCase().includes(searchWord.toLowerCase()) ||
          list.website.toLowerCase().includes(searchWord.toLowerCase()) ||
          list.company?.name.toLowerCase().includes(searchWord.toLowerCase())
        )
      })
    },
  },
  mounted() {
    this.searchApi()
  }
}
</script>

<style>
/* ====== Dark UI Tokens ====== */
:root {
  --bg: #0b0c10;
  --panel: #111318;
  --panel-2: #0f1116;
  --text: #e6e8ee;
  --muted: #a9b1c3;
  --primary: #6ea8ff;
  --primary-2: #3e7bff;
  --ring: rgba(110, 168, 255, 0.35);
  --card: #151821;
  --card-hover: #171b25;
  --border: #232838;
}

html, body {
  margin: 0;
  padding: 0;
  background: radial-gradient(1200px 600px at 10% 0%, #0e1322 0%, var(--bg) 60%);
  color: var(--text);
  font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Noto Sans, 'Helvetica Neue', Arial, 'Apple Color Emoji', 'Segoe UI Emoji';
}

/* ====== Page Container ====== */
body > #app > div,
body > div {
  max-width: 1100px;
  margin: 0 auto;
  padding: 32px 16px 64px;
  box-sizing: border-box;
}

/* ====== Header (pseudo) ====== */
body > #app > div::before,
body > div::before {
  content: "Users Directory";
  display: block;
  font-size: 28px;
  letter-spacing: .4px;
  margin-bottom: 8px;
}
body > #app > div::after,
body > div::after {
  content: "JSONPlaceholder /users を検索";
  display: block;
  color: var(--muted);
  margin-bottom: 16px;
  font-size: 14px;
}

/* ====== Search Bar ====== */
body > #app > div > div:first-child,
body > div > div:first-child {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 10px;
  align-items: center;
  background: linear-gradient(180deg, var(--panel) 0%, var(--panel-2) 100%);
  border: 1px solid var(--border);
  box-shadow: 0 0 0 1px rgba(255,255,255,0.02) inset, 0 10px 30px rgba(0,0,0,0.35);
  border-radius: 14px;
  padding: 12px;
  position: sticky;
  top: 12px;
  z-index: 1;
}

body > #app > div input[type="text"],
body > div input[type="text"] {
  width: 100%;
  background: #0f1320;
  color: var(--text);
  border: 1px solid #1f2434;
  border-radius: 10px;
  padding: 12px 14px;
  outline: none;
  font-size: 14px;
  transition: box-shadow .15s ease, border-color .15s ease;
}
body > #app > div input[type="text"]:focus,
body > div input[type="text"]:focus {
  border-color: var(--primary);
  box-shadow: 0 0 0 4px var(--ring);
}

body > #app > div button,
body > div button {
  background: linear-gradient(180deg, var(--primary) 0%, var(--primary-2) 100%);
  color: white;
  border: none;
  border-radius: 10px;
  padding: 10px 14px;
  font-weight: 600;
  cursor: pointer;
  transition: transform .08s ease, filter .15s ease;
}
body > #app > div button:hover,
body > div button:hover { filter: brightness(1.05); }
body > #app > div button:active,
body > div button:active { transform: translateY(1px); }

/* ====== Empty ====== */
body > #app > div > div:nth-of-type(2),
body > div > div:nth-of-type(2) {
  margin-top: 16px;
  background: linear-gradient(180deg, var(--panel) 0%, var(--panel-2) 100%);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 20px;
  color: var(--muted);
  text-align: center;
}

/* ====== Card List ====== */
ul {
  list-style: none;
  padding: 0;
  margin-top: 24px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 16px;
}

.card {
  background: linear-gradient(180deg, var(--card) 0%, var(--card-hover) 100%);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 16px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.25);
  transition: transform .15s ease, box-shadow .25s ease, border-color .2s ease;
}
.card:hover {
  transform: translateY(-3px);
  box-shadow: 0 18px 40px rgba(0,0,0,0.35);
  border-color: #2f3754;
}

/* ====== Header in Card ====== */
.card__header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 12px;
}
.avatar {
  width: 46px;
  height: 46px;
  border-radius: 12px;
  display: grid;
  place-items: center;
  background: radial-gradient(100% 100% at 30% 30%, #3044ff 0%, #9128ff 100%);
  font-weight: 700;
  color: white;
  font-size: 18px;
}
.info {
  flex: 1;
  overflow: hidden;
}
.name {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  color: var(--text);
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
.email {
  margin: 0;
  font-size: 13px;
  color: var(--muted);
  word-break: break-all;
}

/* ====== Body in Card ====== */
.card__body {
  display: flex;
  flex-direction: column;
  gap: 6px;
  font-size: 14px;
}
.website {
  color: var(--primary);
  text-decoration: none;
  font-weight: 500;
  overflow-wrap: anywhere;
}
.website:hover {
  text-decoration: underline;
}
.company {
  color: #aab4cc;
  background: #1c2234;
  border-radius: 8px;
  padding: 4px 8px;
  font-size: 12px;
  display: inline-block;
  width: fit-content;
}

/* ====== Focus visible ====== */
button:focus-visible,
input:focus-visible,
a:focus-visible {
  outline: none;
  box-shadow: 0 0 0 4px var(--ring);
  border-radius: 10px;
}
</style>
