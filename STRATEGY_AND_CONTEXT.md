# Context & Strategy: Peruquois.com Website Modernization (June 2026)

## 1. Project Overview & Current State
*   **Target Website:** `http://peruquois.com/` (Landing page for a premium music artist, concerts, and women's spiritual practices).
*   **Current Visual State:** Outdated layout ("70s style"), lacking proper spacing, visual hierarchy, and structural alignment. Heavy center-alignment of long texts causes visual fatigue, and crimson buttons are squeezed tight without padding.
*   **Current Tech Stack:** Built on an obsolete, self-made engine using **Parser 3** (a legacy server-side scripting language by Artemy Lebedev Studio). Hosted in Russia on **VK Cloud Solutions (VKCS)** (IP: `95.163.181.103`).
*   **The Problem:** High user drop-off, broken typography (Oswald/Roboto), and major access timeouts/slowdowns for users browsing with a VPN or from outside Russia.
*   **The Goal:** Redesign the landing pages into a premium, fast, and high-converting "magazine-style minimalism" layout using **pure HTML/CSS/JS (Tailwind/Modern CSS)**, completely bypassing the backend script overhead for static blocks and caching everything via a CDN.
*   **Budget:** 100,000 RUB for the core redesign and front-end optimization.

---

## 2. Technical Audit & The VPN Breakdown

Our technical audit confirmed the following performance and SEO bottlenecks:

### A. The Parser vs. VPN Issue (Confirmed)
1.  **Dynamic Generation Overhead:** Parser 3 compiles templates and executes server-side scripts on every single request.
2.  **High Latency & TTFB:** When testing from Moscow (local connection to VKCS), the connection latency is a tiny 4ms, yet the **Time to First Byte (TTFB) is 2.1 seconds**. This is extremely slow and indicates heavy database/script compilation overhead or queue bottlenecks.
3.  **VPN Routing timeouts:** Instagram and Telegram users browse through VPNs (usually routed through Germany/Netherlands). The routing path becomes: *User (RU) ➡️ VPN Server (EU) ➡️ VKCS Server (RU)*. Because of the 2.1s TTFB and network hops, Parser scripts frequently time out, causing `504 Gateway Timeout` errors or blank screens.
4.  **German Proxy overhead:** To bypass blocks, the client uses a proxy tunnel in Germany. This is an unnecessary cost and fails to fix the root cause (slow server-side page rendering).

### B. Core SEO & Indexing Flaws
*   **Missing standard files:** Both `robots.txt` and `sitemap.xml` return a **404 Not Found** error. This is a critical SEO issue. Traditional search engines and AI search crawlers (like GPTBot, ClaudeBot, PerplexityBot) cannot discover pages systematically.
*   **Zero Structured Data:** The site contains **0 JSON-LD schemas** and **0 Microdata elements**. Search engines cannot display Rich Snippets for events, and AI search engines cannot easily parse Peruquois' profile (`Person`), courses (`Course`), or concerts (`Event`).
*   **Header issues:** Multiple `<h1>` tags are present on the same page, breaking heading hierarchy guidelines.

### C. The Solution: Pure HTML + CDN (Cloudflare)
*   Deploy pages as **static, pure HTML/CSS/JS**.
*   Route traffic through a CDN like **Cloudflare**. The nearest European CDN node will serve the static HTML instantly (~50ms) to a user on a VPN, bypassing the VKCS server in Moscow entirely.

---

## 3. UI/UX Critique & Redesign Requirements

The modernized layout will adopt a premium, clean "magazine-style minimalism" aesthetic (Vogue, Kinfolk, Apple HIG style) optimized for conversions.

### Key Visual & Layout Fixes:
*   **Hero Section:** Replace the cold, dark video player box at the top with a full-bleed, high-quality, atmospheric photo of Peruquois. Add a soft background glow representing "energy movement." Put the video behind a styled "Play Video" overlay button.
*   **Typography Pairing:**
    *   *Headings:* Replace the blocky, industrial **Oswald** with **Forum** (an elegant, ancient Roman proportioned serif font with beautiful Cyrillic support).
    *   *Body text:* Replace the mechanical **Roboto** with **Tenor Sans** (a clean, luxury-fashion humanist sans-serif).
*   **Alignment:** Change body paragraphs from center-alignment to left-alignment to eliminate jagged left edges and visual fatigue. Break paragraphs into short 3-4 line blocks.
*   **Spacing (Fitts's Law):** Add generous margins and paddings (e.g., `py-24` or `40-60px` margins around CTAs) to let elements breathe.
*   **Social Proof:** Align celebrity reviews (Irina Khakamada, Anfisa Chekhova) in structured layouts. Use a warm, contrasting background container (light sand, champagne, or off-white) to set them apart.
*   **Alternating Backgrounds:** Break the monotony of the vertical white scroll by alternating background tones: clean whites/warm light-cream for course details and testimonials, and deep immersive dark themes (concert hall vibe) for performance and music blocks.

---

## 4. Prompt Engineering Directives for the Agent

1.  **Strict Styling Guideline:** Rely on **white space** as a key design element. Ensure generous spacing (`py-24`, `gap-12`).
2.  **No Heavy JS/Frameworks:** Keep the code footprint under 30KB. Use standard responsive CSS Grids/Flexbox and pure CSS keyframe animations.
3.  **Clean HTML Outputs:** Provide valid, self-contained HTML/CSS that is easy to deploy on any server or CDN node without requiring Parser script modifications.
