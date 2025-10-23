# Web Context Protocol (WCP)

**A standard for developers to control how AI agents interact with their web pages.**

## The Problem

As AI browsers emerge (ChatGPT Atlas, Perplexity Comet, Arc AI, Anthropic Computer Use), developers have **no control** over how AI agents interact with their sites.

Right now, AI agents are guessing:
- They see your page, find multiple buttons, and make their best guess based on text and CSS classes
- They might click "Subscribe to Newsletter" when the user says "add to cart"
- They might navigate away when you expected an async update
- You have no way to tell them what you intended

**This is a developer control problem, not just an AI accuracy problem.**

Just like we have:
- **ARIA** for screen readers
- **Open Graph** for social media
- **Schema.org** for search engines

We need **WCP** for AI agents.

---

## The Solution

WCP is a simple HTML standard that lets **you** tell AI agents how to interact with **your** page:

```html
<button data-wcp="action:add; effect:async; primary:true">
  Add to Cart
</button>

<button data-wcp="action:purchase; effect:navigate">
  Checkout
</button>
```

Now you control:
- ✅ What each element does (semantic action)
- ✅ How it behaves (side effects)
- ✅ Which actions are primary
- ✅ Where updates appear

This isn't about making AI smarter—it's about **giving developers control over their own sites.**

---

## Quick Start

### E-commerce Product Page
```html
<div data-wcp="region:product">
  <!-- Primary action: adds to cart, updates async -->
  <button data-wcp="action:add; effect:async; target:#cart; primary:true">
    Add to Cart
  </button>
  
  <!-- Secondary action: goes straight to checkout -->
  <button data-wcp="action:purchase; effect:navigate">
    Buy Now
  </button>
  
  <!-- Another secondary action: async update -->
  <button data-wcp="action:add; effect:async">
    Add to Wishlist
  </button>
</div>

<!-- Cart widget that gets updated -->
<div id="cart" data-wcp="region:checkout">
  <span>Items: 0</span>
</div>
```

### Search Interface
```html
<form data-wcp="region:search; action:search; effect:async; target:#results">
  <input type="search" name="q" />
  <button data-wcp="action:search; effect:async; primary:true">
    Search
  </button>
</form>

<div id="results" data-wcp="region:results">
  <!-- Search results appear here -->
</div>
```

### Settings Form
```html
<form data-wcp="action:submit; effect:async; target:#notification">
  <!-- Primary action: saves without navigation -->
  <button type="submit" data-wcp="action:submit; effect:async; primary:true">
    Save Changes
  </button>
  
  <!-- Secondary action: navigates back -->
  <button type="button" data-wcp="action:navigate; effect:navigate">
    Cancel
  </button>
</form>

<div id="notification"></div>
```

---

## Core Vocabulary

### Actions (What it does)
| Action | Use Case | Example |
|--------|----------|---------|
| `add` | Adding items | Add to cart, save to list |
| `remove` | Removing items | Delete from cart, remove tag |
| `purchase` | Initiating payment | Checkout, buy now |
| `search` | Search operations | Search bar, filters |
| `filter` | Filtering content | Category filters, sort |
| `submit` | Form submission | Save settings, send message |
| `navigate` | Navigation | Links, menu items |
| `auth` | Authentication | Login, signup, logout |
| `custom.*` | Domain-specific | Your custom actions |

### Effects (How it behaves)
| Effect | Behavior | Use When |
|--------|----------|----------|
| `async` | Updates page without reload | AJAX, live updates |
| `navigate` | Full page navigation | Links, form submissions |
| `modal` | Opens overlay/dialog | Modals, popovers |
| `download` | Triggers file download | PDFs, exports |
| `external` | Leaves current site | External links |

### Regions (Semantic zones)
| Region | Purpose |
|--------|---------|
| `navigation` | Site navigation, menus |
| `content` | Main page content |
| `product` | Product details (e-commerce) |
| `checkout` | Checkout flow |
| `search` | Search interface |
| `results` | Search/filter results |

### Modifiers
- `primary:true` - Mark the primary action in each region/context
- `target:#selector` - Specify where async updates appear

---

## Before & After Comparison

### ❌ Without WCP (AI guessing)
```html
<button class="btn btn-primary" onclick="addToCart()">
  Add to Cart
</button>
<button class="btn btn-primary" onclick="subscribe()">
  Subscribe to Updates
</button>
<button class="btn btn-primary" onclick="share()">
  Share Product
</button>
```

**What AI agent sees:**
- Three identical-looking buttons
- No semantic meaning
- Must guess based on text alone

### ✅ With WCP (developer control)
```html
<button data-wcp="action:add; effect:async; primary:true" 
        onclick="addToCart()">
  Add to Cart
</button>
<button data-wcp="action:auth; effect:modal" 
        onclick="subscribe()">
  Subscribe to Updates
</button>
<button data-wcp="action:navigate; effect:modal" 
        onclick="share()">
  Share Product
</button>
```

**What AI agent sees:**
- First button: adds to cart, async update, primary action ✅
- Second button: auth action, opens modal
- Third button: navigation action, opens modal
- 95%+ accuracy

---

## Design Principles

**Simple:** One attribute (`data-wcp`), not multiple competing standards

**Extensible:** Use `custom.*` namespace for domain-specific actions
```html
<button data-wcp="action:custom.schedule-meeting; effect:modal">
  Schedule Demo
</button>
```

**Optional:** Works alongside existing HTML, doesn't break anything

**Open:** CC0 license (public domain) - no strings attached

**Backwards compatible:** Browsers/agents that don't understand WCP safely ignore it

---

## Progress Status

- 🚧 Production-ready parser for browser integration

**We're seeking feedback before finalizing v1.0.**

---

## For AI Browser Developers

If you're building an AI browser or agent system, WCP provides:

1. **Reliable action detection** - No more guessing which button does what
2. **Behavior prediction** - Know if clicking will navigate, open modal, or update async
3. **Priority signals** - Identify primary actions automatically
4. **Target tracking** - Wait for the right elements to update

### Quick Integration

```javascript
// Find primary "add to cart" action
const elements = document.querySelectorAll('[data-wcp]');
const addButton = Array.from(elements).find(el => {
  const wcp = el.getAttribute('data-wcp');
  return wcp.includes('action:add') && wcp.includes('primary:true');
});

// Parse WCP annotation
const attrs = {};
addButton.getAttribute('data-wcp').split(';').forEach(pair => {
  const [key, value] = pair.split(':').map(s => s.trim());
  attrs[key] = value === 'true' ? true : value;
});

// Now you know:
// attrs.action => "add"
// attrs.effect => "async"
// attrs.primary => true
// attrs.target => "#cart"

// Execute with confidence
if (attrs.effect === 'async') {
  addButton.click();
  // Wait for attrs.target to update
  await waitForElement(attrs.target);
}
```

Reference implementation: [`wcp-executor.js`](./examples/wcp-executor.js)

---

## FAQ

**Q: Why not just use ARIA?**  
A: ARIA is for accessibility (screen readers). WCP is for AI agents. Different use case, different vocabulary.

**Q: Why not use better AI vision?**  
A: Vision is expensive, slow, and brittle. WCP is instant, free, and deterministic. Use vision as fallback, WCP as primary.

**Q: What if sites lie in their annotations?**  
A: Sites that lie break AI agents on their own site—hurting themselves, not users. Natural incentive alignment.

**Q: Will AI browsers actually support this?**  
A: That's what we're figuring out! We're reaching out to browser teams. Early conversations are positive.

**Q: Can I use custom actions?**  
A: Yes! Use the `custom.*` namespace:
```html
<button data-wcp="action:custom.schedule-demo; effect:modal">
```

**Q: How is this different from MCP?**  
A: MCP (Model Context Protocol) connects LLMs to tools/data. WCP helps agents understand web pages. Different layers.

---

## Contributing

We're actively seeking feedback on:
- Missing actions or effects
- Real-world use cases we haven't considered
- Integration challenges for browser developers
- Edge cases in the spec

**Open an issue or PR!** All contributions welcome.

---

## Roadmap

**v1.0 (Current)**
- ✅ Core specification
- ✅ Basic examples
- ✅ Reference implementation

**v1.1 (Next)**
- [ ] Form field constraints (`field:email; required:true; format:standard`)
- [ ] Validation hints for AI agents
- [ ] Extended region vocabulary
- [ ] Browser extension for testing

**v2.0 (Future)**
- [ ] Dynamic action discovery
- [ ] Conditional annotations
- [ ] Multi-step workflow support
- [ ] Analytics/telemetry standard

---

## Resources

- **Documentation:** [wcp.dev](https://wcp.dev)
---

## License

**CC0 1.0 Universal (Public Domain)**

The Web Context Protocol specification is released under CC0. Anyone can implement, extend, or modify it without restriction. This is infrastructure, not a product.

---

## About

Built by [Umbrella Mode](https://umbrellamode.com) (David Osemwegie, [@osazi_)](https://x.com/osazi_))

Open to contributions from the entire community. This is a **standard**, not a company product.

---

## Why Now?

ChatGPT just released Atlas (AI browser). Perplexity announced Comet (AI browser). Arc has AI features. Anthropic has Computer Use. The AI browser wave is here.

**Developers need control. WCP provides it.**
