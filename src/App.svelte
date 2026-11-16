
<script lang="ts">
  type Message = {
    id: number;
    author: string;
    avatar: string;
    content: string;
    time: string;
    own?: boolean;
  };

  const servers = [
    { id: "kestrel", name: "Kestrel Community", icon: "K", color: "#5865f2" },
    { id: "dev", name: "Developers Hub", icon: "D", color: "#168b75" },
    { id: "gaming", name: "Gaming Lounge", icon: "G", color: "#a66be0" }
  ];

  const channels = [
    { id: "general", name: "general", category: "TEXT CHANNELS" },
    { id: "welcome", name: "welcome", category: "TEXT CHANNELS" },
    { id: "projects", name: "projects", category: "TEXT CHANNELS" },
    { id: "announcements", name: "announcements", category: "INFORMATION" }
  ];

  let selectedServer = "kestrel";
  let selectedChannel = "general";
  let draft = "";

  let messages: Message[] = [
    {
      id: 1,
      author: "Kestrel Bot",
      avatar: "K",
      content: "Welcome to Kestrel! This is your local demo workspace. 🪶",
      time: "09:30"
    },
    {
      id: 2,
      author: "Alex",
      avatar: "A",
      content: "Hey everyone! Welcome to our community.",
      time: "09:32"
    },
    {
      id: 3,
      author: "Morgan",
      avatar: "M",
      content: "The Kestrel project is officially taking flight!",
      time: "09:35"
    }
  ];

  function sendMessage(event: SubmitEvent) {
    event.preventDefault();

    const content = draft.trim();

    if (!content) return;

    messages = [
      ...messages,
      {
        id: Date.now(),
        author: "You",
        avatar: "Y",
        content,
        time: new Date().toLocaleTimeString([], {
          hour: "2-digit",
          minute: "2-digit"
        }),
        own: true
      }
    ];

    draft = "";
  }
</script>

<svelte:head>
  <title>Kestrel — Community</title>
  <meta
    name="description"
    content="Kestrel lightweight community desktop client."
  />
</svelte:head>

<div class="app-shell">
  <aside class="server-sidebar">
    <div class="brand-mark">K</div>

    <div class="sidebar-divider"></div>

    {#each servers as server}
      <button
        class:active={selectedServer === server.id}
        class="server-button"
        style:--server-color={server.color}
        title={server.name}
        onclick={() => {
          selectedServer = server.id;
          selectedChannel = "general";
        }}
      >
        {server.icon}
      </button>
    {/each}

    <button class="add-server" title="Demo action">+</button>

    <div class="sidebar-bottom">
      <div class="profile-avatar">Y</div>
    </div>
  </aside>

  <aside class="channel-sidebar">
    <div class="workspace-header">
      <span>{servers.find((s) => s.id === selectedServer)?.name}</span>
      <span class="workspace-arrow">⌄</span>
    </div>

    <div class="channel-list">
      <div class="channel-category">TEXT CHANNELS</div>

      {#each channels.filter((c) => c.category === "TEXT CHANNELS") as channel}
        <button
          class:channel-active={selectedChannel === channel.id}
          class="channel-button"
          onclick={() => (selectedChannel = channel.id)}
        >
          <span class="hash">#</span>
          {channel.name}
        </button>
      {/each}

      <div class="channel-category information-category">
        INFORMATION
      </div>

      {#each channels.filter((c) => c.category === "INFORMATION") as channel}
        <button
          class:channel-active={selectedChannel === channel.id}
          class="channel-button"
          onclick={() => (selectedChannel = channel.id)}
        >
          <span class="hash">#</span>
          {channel.name}
        </button>
      {/each}
    </div>

    <div class="user-panel">
      <div class="profile-avatar">Y</div>
      <div class="user-details">
        <strong>You</strong>
        <span>Local Demo</span>
      </div>
      <span class="user-settings">⚙</span>
    </div>
  </aside>

  <main class="chat-area">
    <header class="chat-header">
      <div class="chat-title">
        <span class="hash">#</span>
        <strong>{selectedChannel}</strong>
      </div>
      <div class="demo-indicator">
        <span class="status-dot"></span>
        DEMO MODE
      </div>
    </header>

    <section class="messages-area">
      <div class="welcome-banner">
        <div class="welcome-icon">#</div>
        <h1>Welcome to #{selectedChannel}!</h1>
        <p>This is the beginning of this channel.</p>
      </div>

      {#each messages as message (message.id)}
        <article class="message">
          <div class:own-avatar={message.own} class="message-avatar">
            {message.avatar}
          </div>

          <div class="message-content">
            <div class="message-meta">
              <strong>{message.author}</strong>
              <time>{message.time}</time>
            </div>
            <p>{message.content}</p>
          </div>
        </article>
      {/each}
    </section>

    <footer class="composer-area">
      <form class="message-form" onsubmit={sendMessage}>
        <button
          type="button"
          class="composer-plus"
          title="Demo attachment"
        >+</button>

        <input
          bind:value={draft}
          placeholder="Message #{selectedChannel}"
          aria-label="Write a message"
        />

        <button
          type="submit"
          class="send-button"
          disabled={!draft.trim()}
          aria-label="Send message"
        >➤</button>
      </form>

      <div class="demo-notice">
        Messages are stored only in this local demo session.
      </div>
    </footer>
  </main>
</div>
