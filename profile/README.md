# Web Context Protocol (WCP)
**A proposed HTML standard for controlling AI agents in browsers**

As more users adopt AI-powered browsers, developers need a way to **control how LLMs interact with their pages** — safely and predictably.

---

## 🧩 The Problem

Right now, AI agents are guessing.

They look at your page, see multiple buttons, and make their best guess based on text and CSS classes — no different than a user clicking randomly.

As a developer, you have no control over what the AI does. It might:
- Click “Subscribe to Newsletter” when the user says “add to cart”
- Submit a form when the user meant to cancel
- Navigate away when you expected an async update

This isn’t just an AI accuracy issue — it’s a **developer control problem**.

---

## 💡 The Solution — Web Context Protocol (WCP)

A simple HTML standard that lets **you** tell AI agents how to interact with **your** page.

```html
<button data-wcp="action:add; effect:async; primary:true">
  Add to Cart
</button>

<button data-wcp="action:purchase; effect:navigate">
  Checkout
</button>
```

Now you control:
- **What** each element does (`action:add`, `purchase`, `search`, `submit`, etc.)
- **How** it behaves (`effect:async`, `navigate`, `modal`, `download`, etc.)
- **Which** actions are primary

Example outside of e-commerce:
```html
<button data-wcp="action:delete; effect:modal">Delete Project</button>
```

---

## ⚙️ Why This Matters *Now*

AI browsers are already here:
- **ChatGPT Atlas** just launched  
- **Perplexity Comet** announced a few weeks ago  
- AI browsers like [DIA](https://www.diabrowser.com) and [StrawBerry](https://strawberrybrowser.com) are out in the wild.
- 

Just like we have **ARIA** for accessibility and **Open Graph** for social sharing, we need a standard for **AI interactions**.

This isn’t about making AI smarter — it’s about giving **developers control over their own sites**.

---

## 🧱 Design Principles

- **Simple:** One attribute (`data-wcp`)
- **Extensible:** Support custom actions for your domain
- **Optional:** Works alongside existing HTML
- **Open:** CC0 license (public domain)

---

## 🚧 Roadmap

- [ ] Define initial schema and parser
- [ ] Draft `@webcontextprotocol/dom` JavaScript library
- [ ] Build Chrome/Atlas/Comet polyfill
- [ ] Publish spec v0.1
- [ ] Establish WCP Working Group (Discord + GitHub)

---

## 💬 Get Involved

I'm sharing this early to get feedback **before** building out the full parser and browser integrations.

**What I need:**
- Is this the right approach?
- What actions/effects are missing?
- What would you change?
- Connections to AI browser teams (Perplexity, Arc, Browser Company)

**Join the discussion:**
- **GitHub:** [github.com/webcontextprotocol](https://github.com/webcontextprotocol)
- **Discussions:** [GitHub Discussions](https://github.com/web-context-protocol/spec/discussions)
- **Discord:** [Join the WCP Developer Community](https://discord.gg/cGfZmVA7)

---

### 👤 Author

**David Osemwegie**  
Founder – [UmbrellaMode](https://umbrellamode.com)  
[@osazi_ on Twitter](https://twitter.com/osazi_)

> If you care about how AI agents use your site, join the discussion on GitHub and Discord.  
> Let’s define this standard *before* it’s defined for us.

---
