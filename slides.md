---
title: 'TanStack Start — クライアントファーストという選択'
theme: default
layout: cover
background: '#FDFDFD'
class: text-neutral-900
drawings:
  persist: false
transition: slide-left
mdc: true
highlighter: shiki
fonts:
  sans: 'Inter'
  mono: 'JetBrains Mono'
  weights: '400,500,600,700,800,900'
colorSchema: 'light'
author: tsuboi
---

<div class="absolute top-20 right-20 w-64 h-64 rounded-full bg-cyan-500/5"></div>
<div class="absolute bottom-16 left-12 w-40 h-40 rounded-lg bg-amber-500/10 rotate-12"></div>

<div class="flex flex-col items-start justify-center h-full gap-6">

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600">LT Talk · 2026</p>

<h1 class="text-8xl font-black tracking-tighter leading-none">
  TANSTACK
  <span class="inline-block text-transparent bg-clip-text bg-gradient-to-r from-teal-500 to-cyan-500">
    START
  </span>
</h1>

<p class="text-2xl font-medium text-neutral-700 max-w-3xl">
  クライアントファーストという選択
</p>

<p class="text-base text-neutral-500 max-w-2xl">
  SSR 全盛の時代に、なぜ Router-first / Client-first なのか
</p>

<div class="text-sm text-neutral-500 font-medium mt-6">— tsuboi</div>

</div>

<style>
  .slidev-layout {
    padding: 3rem 4rem;
    background: #FDFDFD;
    color: #171717;
  }
  .slidev-layout h1, .slidev-layout h2, .slidev-layout h3 { letter-spacing: -0.02em; }
  .slidev-layout code {
    background: #F3F4F6;
    padding: 0.15rem 0.4rem;
    border-radius: 0.375rem;
    font-size: 0.9em;
  }
</style>

<!--
オープニング。SSR / RSC が "正解" とされる空気の中で、あえて client-first を選ぶ理由を 15–20 分で。
聴衆は Next.js App Router を触ったことがある人、`'use client'` の置き場所で悩んだことがある人を想定。
持ち帰ってほしいのは「Start = Next.js 対抗ではなく、Router-first / Client-first というもう一つの軸」。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Agenda</p>

# <span class="text-4xl font-extrabold tracking-tight">今日話すこと</span>

<div class="grid grid-cols-2 gap-6 mt-10">

<v-clicks>

<div class="bg-cyan-50 p-6 rounded-lg transition-all duration-200 hover:scale-[1.02] hover:bg-cyan-100">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">01 · Identity</div>
  <div class="text-lg font-bold text-neutral-900">Start ＝ Router である</div>
  <div class="text-sm text-neutral-600 mt-1">独立リポジトリは存在しない、薄い層</div>
</div>

<div class="bg-neutral-900 p-6 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-400 mb-2">02 · Philosophy</div>
  <div class="text-lg font-bold text-white">Client-first 哲学</div>
  <div class="text-sm text-neutral-300 mt-1">公式が CRITICAL と明言する設計思想</div>
</div>

<div class="bg-amber-50 p-6 rounded-lg transition-all duration-200 hover:scale-[1.02] hover:bg-amber-100">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600 mb-2">03 · Robustness</div>
  <div class="text-lg font-bold text-neutral-900">堅牢性の源泉</div>
  <div class="text-sm text-neutral-600 mt-1">ドーナツホール問題と createServerFn</div>
</div>

<div class="bg-purple-50 p-6 rounded-lg transition-all duration-200 hover:scale-[1.02] hover:bg-purple-100">
  <div class="text-xs font-semibold tracking-widest uppercase text-purple-600 mb-2">04 · When</div>
  <div class="text-lg font-bold text-neutral-900">いつ選ぶか</div>
  <div class="text-sm text-neutral-600 mt-1">Next.js / Remix との使い分け</div>
</div>

</v-clicks>

</div>

<!-- 
Start を Next.js の対抗ではなく「Router をフルスタックに拡張した薄い層」として位置づける。
Zenn 記事 https://zenn.dev/tsuboi/articles/0d2d63b584aa2c の論点 (donut hole / RSC批判) を 03 で接続。
-->

---
layout: section
class: 'bg-cyan-500 text-white'
---

<div class="absolute top-10 right-10 w-48 h-48 rounded-full bg-white/5"></div>
<div class="absolute bottom-10 left-10 w-32 h-32 rounded-lg bg-white/10 rotate-45"></div>

<p class="text-sm font-semibold tracking-widest uppercase opacity-80 mb-4">Section 01 · Identity</p>

# <span class="text-7xl font-black tracking-tighter">Start = Router-powered</span>

<p class="text-xl font-medium mt-6 opacity-90 max-w-3xl">
  独立リポジトリではない。Router の上に乗った full-stack framework
</p>

<!--
セクション 01。ここから 5 スライドで「Start は独立フレームワークではなく、Router の上に被さった薄い層」を事実で示す。
最初に Identity (= 何者か) を確定させると、後の Client-first / 設計判断の話が腹落ちしやすい。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Fact 01</p>

# <span class="text-4xl font-extrabold tracking-tight"><code class="text-cyan-600 font-mono">TanStack/start</code> は存在しない</span>

<p class="text-sm text-neutral-600 mt-2">独立リポジトリを叩くと <code>404 Not Found</code> が返る。Start の開発は <code>TanStack/router</code> 内で進められている。</p>

<div class="grid grid-cols-2 gap-4 mt-3">

<div class="bg-neutral-900 text-white p-4 rounded-lg font-mono text-xs">
<div class="text-xs font-semibold tracking-widest uppercase text-red-400 mb-1">github.com/TanStack/start</div>

```text
404
Page not found
```

<div class="text-xs text-neutral-400 font-sans mt-2">→ Start という独立リポジトリは存在しない</div>

</div>

<div class="bg-cyan-50 p-4 rounded-lg font-mono text-xs">
<div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-1">github.com/TanStack/router</div>

```text
description: "🤖 A client-first,
  server-capable, fully type-safe
  router and full-stack framework
  for the web (React and more)."

stars: 14.5k+  forks: 1.7k+
```

<div class="text-xs text-neutral-500 font-sans mt-2">→ 自己紹介に "client-first" / Start は <span class="font-semibold">React と Solid を第一級サポート</span></div>

</div>

</div>

<v-click>

<p class="mt-3 text-base font-bold text-neutral-900">
  公式の自己定義からして <span class="text-cyan-600">"client-first"</span> が含まれている。
</p>

</v-click>

<v-click>

<div class="grid grid-cols-3 gap-3 mt-3 text-center">
  <div class="bg-emerald-50 p-2 rounded-lg">
    <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">react-router/src</div>
    <div class="text-xl font-black text-emerald-500 mt-0.5">45 <span class="text-xs font-bold">files</span> · 187<span class="text-xs">KB</span></div>
  </div>
  <div class="bg-cyan-50 p-2 rounded-lg">
    <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">react-start/src</div>
    <div class="text-xl font-black text-cyan-500 mt-0.5">12 <span class="text-xs font-bold">files</span> · 2.5<span class="text-xs">KB</span></div>
  </div>
  <div class="bg-neutral-900 text-white p-2 rounded-lg">
    <div class="text-xs font-semibold tracking-widest uppercase opacity-60">逆算</div>
    <div class="text-xl font-black text-amber-400 mt-0.5">Router ≒ Start の <span class="text-cyan-400">73 倍</span></div>
  </div>
</div>

<p class="mt-2 text-sm text-neutral-700">
  → src 実測でも Start は Router の <span class="font-bold text-cyan-600">1.4%</span>。<span class="font-bold">"Start の 90% は Router"</span> は誇張ではなく実測。
</p>

</v-click>

<!--
1 つ目の事実。`github.com/TanStack/start` は実在しない (404)。Start の開発は `TanStack/router` リポジトリ内で進む。
左：404 のキャプチャ / 右：`TanStack/router` の description にすでに "client-first" / "server-capable" / "full-stack framework" と書かれている。
"Start の 90% は Router" は感覚値ではなく実測 — `react-router/src` (45 files, 187KB) vs `react-start/src` (12 files, 2.5KB) で 73 倍差。
LT で 1 番のつかみ。「Start は薄い層」という主張の物量的裏付け。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Fact 02</p>

# <span class="text-4xl font-extrabold tracking-tight">TanStack の "売り"</span>

<div class="grid grid-cols-2 gap-4 mt-5">

<div class="bg-emerald-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 mb-2">TanStack Router</div>
  <div class="text-sm text-neutral-700 leading-snug">
    "A modern router designed for <span class="font-bold">type safety</span>, data-driven navigation, and seamless developer experience."
  </div>
  <div class="text-xs text-neutral-500 mt-1 leading-snug">
    → 型安全性とデータ駆動ナビゲーションを軸に、開発体験を磨いたモダンなルーター
  </div>
  <ul class="text-xs text-neutral-600 mt-2 space-y-0.5">
    <li>· End-to-end type safety <span class="text-neutral-400">／ 端まで型安全</span></li>
    <li>· Schema-driven search params <span class="text-neutral-400">／ スキーマで検証する URL クエリ</span></li>
    <li>· Built-in caching / prefetching <span class="text-neutral-400">／ キャッシュ・プリフェッチ内蔵</span></li>
    <li>· Nested layouts, transitions <span class="text-neutral-400">／ 入れ子レイアウト・遷移</span></li>
  </ul>
</div>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">TanStack Start</div>
  <div class="text-sm text-neutral-700 leading-snug">
    "A full-stack framework <span class="font-bold">built on Router</span>, designed for server rendering, streaming, and production-ready deployments."
  </div>
  <div class="text-xs text-neutral-500 mt-1 leading-snug">
    → Router の上に乗せた、SSR・ストリーミング・本番運用向けのフルスタック層
  </div>
  <ul class="text-xs text-neutral-600 mt-2 space-y-0.5">
    <li>· Full-document SSR &amp; streaming <span class="text-neutral-400">／ ドキュメント全体の SSR と段階送信</span></li>
    <li>· Server functions <span class="text-neutral-400">／ サーバー関数 (RPC)</span></li>
    <li>· Deployment-ready builds <span class="text-neutral-400">／ デプロイ可能なビルド出力</span></li>
    <li>· <span class="font-bold text-cyan-600">All the power of Router + α</span> <span class="text-neutral-400">／ Router の全機能 + α</span></li>
  </ul>
</div>

</div>

<v-click>

<div class="mt-4 p-3 bg-neutral-900 text-white rounded-lg">
  <span class="text-xs font-semibold tracking-widest uppercase opacity-60">README より</span>
  <div class="text-sm font-medium mt-0.5">"All the power of <span class="text-cyan-400">TanStack Router</span>, plus full-stack features."</div>
  <div class="text-xs opacity-70 mt-0.5">→ Router の全機能に、SSR / Server Functions / deployment を足したもの。</div>
</div>

</v-click>

<!--
README の自己定義を引用。Router は "type safety + data-driven navigation"、Start は "Router の上に乗せた full-stack 層"。
キーフレーズは "All the power of TanStack Router, plus full-stack features" — Start は Router の全機能を継承した上で SSR / Server Functions / deployment を足したもの。
=> Router を理解できれば Start の 9 割は理解できる、という構造的な軽さを強調。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Fact 03</p>

# <span class="text-4xl font-extrabold tracking-tight">2 系統の薄いスタックを Vite plugin で繋ぐ</span>

<p class="text-sm text-neutral-600 mt-2">ユーザーは <span class="font-bold">Start 系</span> と <span class="font-bold">Router 系</span> の 2 つを install する。Vite plugin が両者を繋いで 1 つのフルスタック framework になる。外部依存は <code>pathe</code> 1 つだけ。</p>

<div class="grid grid-cols-2 gap-4 mt-4">

<div class="space-y-1.5">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 text-center">Start 系 (server functions)</div>

  <div class="bg-cyan-100 p-2.5 rounded-lg border-l-4 border-cyan-500">
    <code class="text-sm font-bold text-cyan-700">@tanstack/react-start</code>
    <div class="text-xs text-neutral-600 mt-0.5">薄いエントリ — <code>useServerFn</code> + 再エクスポート</div>
  </div>

  <div class="text-center text-neutral-400 text-xs leading-none">↓</div>

  <div class="bg-amber-100 p-2.5 rounded-lg border-l-4 border-amber-500">
    <code class="text-sm font-bold text-amber-700">@tanstack/start-client-core</code>
    <div class="text-xs text-neutral-600 mt-0.5">RPC 機構 — <code>createServerFn</code> / stream</div>
  </div>
</div>

<div class="space-y-1.5">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 text-center">Router 系 (routing)</div>

  <div class="bg-emerald-100 p-2.5 rounded-lg border-l-4 border-emerald-500">
    <code class="text-sm font-bold text-emerald-700">@tanstack/react-router</code>
    <div class="text-xs text-neutral-600 mt-0.5">React bindings — <code>&lt;Link&gt;</code> / hooks</div>
  </div>

  <div class="text-center text-neutral-400 text-xs leading-none">↓</div>

  <div class="bg-neutral-200 p-2.5 rounded-lg border-l-4 border-neutral-500">
    <code class="text-sm font-bold text-neutral-700">@tanstack/router-core</code>
    <div class="text-xs text-neutral-600 mt-0.5">本体 — ルートマッチ / 型推論 / loader</div>
  </div>
</div>

</div>

<div class="text-center text-neutral-400 text-xs mt-2 leading-none">↘ &nbsp; ↙</div>

<div class="bg-neutral-900 text-white p-2.5 rounded-lg mt-1">
  <code class="text-sm font-bold text-cyan-400">@tanstack/start-plugin-core</code>
  <span class="text-xs opacity-80 ml-2">Vite plugin で双方を統合 (route 生成 / SSR / server function 配線)</span>
</div>

<v-click>

<p class="mt-2 text-sm text-neutral-700">
  → <code>@tanstack/react-start</code> 単体で全部入るわけではない。<span class="font-bold text-cyan-600">2 系統 × plugin</span> でフルスタックを構成。
</p>

</v-click>

<!--
レイヤー構造を 2 本柱 + plugin 統合で正確に可視化。

【Start 系】
- @tanstack/react-start: 薄いエントリ (useServerFn + 再エクスポート)
- @tanstack/start-client-core: RPC 機構 (createServerFn / stream)

【Router 系】 (Start 系とは独立した npm package)
- @tanstack/react-router: React 用 bindings (<Link>, hooks)
- @tanstack/router-core: framework-agnostic な本体 (ルートマッチ / 型推論 / loader)

【統合層】
- @tanstack/start-plugin-core: Vite plugin で route 生成 / SSR / server function を配線

ユーザー視点：
- install するのは @tanstack/react-start + @tanstack/react-router の 2 系統
- @tanstack/react-start を import しても、react-router / router-core は transitive に同梱されない
- 1 つの "framework" に見えるのは Vite plugin が繋いでいるから

外部依存は `pathe` 1 つだけ — それ以外は全部 workspace 内 (router monorepo 内で完結)。
依存が極端にミニマルだからこそ Router の進化がそのまま Start に乗る。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Fact 04 · Routing</p>

# <span class="text-4xl font-extrabold tracking-tight">File-Based Routing も Router 由来</span>

<p class="text-sm text-neutral-600 mt-2">
  Start は <code>src/routes</code> を読むが、実体は <span class="font-bold">TanStack Router の route tree generation</span>。dev / start 時に <code>src/routeTree.gen.ts</code> が自動生成され、型推論の中核を担う。
</p>

<div class="grid grid-cols-2 gap-4 mt-3">

<div class="bg-emerald-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 mb-2">Directory Routes</div>

```text
src/routes/
  __root.tsx
  posts/
    route.tsx       -> /posts layout
    index.tsx       -> /posts
    $postId.tsx     -> /posts/$postId
```

</div>

<div class="bg-cyan-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Flat Routes</div>

```text
src/routes/
  __root.tsx
  posts.tsx         -> /posts layout
  posts.index.tsx   -> /posts
  posts.$postId.tsx -> /posts/$postId
```

</div>

</div>

<v-click>

<div class="mt-3 p-3 bg-neutral-900 text-white rounded-lg">
  <span class="text-xs font-semibold tracking-widest uppercase opacity-60">Key</span>
  <div class="text-sm font-medium mt-0.5">どちらも同じ route tree に変換される。<span class="text-cyan-400">混在も可</span>。生成された <code class="text-amber-400">routeTree.gen.ts</code> が型推論を高速・FULLY INFERRED に保つ。</div>
</div>

</v-click>

<!--
File-Based Routing も Router の機能。Start 固有ではない。
`src/routes` から `routeTree.gen.ts` が自動生成され、これが型推論の中核。FULLY INFERRED の正体はこのファイル。
Directory 形式と Flat 形式が混在可能。チームの好み / コロケーション戦略で選べる。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Code · The Smoking Gun</p>

# <span class="text-4xl font-extrabold tracking-tight"><code>@tanstack/react-start</code> のソースは <span class="text-cyan-600">2 行</span></span>

<p class="text-sm text-neutral-600 mt-2">前ページで「薄いエントリ」と書いた根拠。<code>react-start/src/index.ts</code> の <span class="font-bold">全文</span>がこれ。</p>

```ts {all|1|2|all}{lines:true}
export { useServerFn } from './useServerFn'
export * from '@tanstack/start-client-core'
```

<div class="grid grid-cols-2 gap-4 mt-4">

<v-click>

<div class="bg-cyan-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-1">L1 — 唯一の "実コード"</div>
  <div class="text-xs text-neutral-700"><code>useServerFn</code> フック (前スライドで触れた、コンポーネントから server function を呼ぶ用)。中身は <span class="font-bold">たった 35 行</span>。</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 mb-1">L2 — 残り全部</div>
  <div class="text-xs text-neutral-700"><code>createServerFn</code> / <code>createMiddleware</code> 等の public API は、下のレイヤー <code>start-client-core</code> から<span class="font-bold">丸ごと再エクスポート</span>。</div>
</div>

</v-click>

</div>

<v-click>

<div class="mt-3 p-3 bg-neutral-900 text-white rounded-lg">
  <span class="text-xs font-semibold tracking-widest uppercase opacity-60">つまり</span>
  <div class="text-base font-bold mt-1"><code class="text-cyan-400">@tanstack/react-start</code> は <span class="text-cyan-400">start-client-core の React adapter</span> にすぎない。<span class="opacity-80 font-medium">→ Solid 用 (<code class="text-amber-300">solid-start</code>) も同じ薄さで成立する理由。</span></div>
</div>

</v-click>

<!--
決定的証拠。`@tanstack/react-start` のメインエントリ `src/index.ts` はたった 2 行。
1 行目：`useServerFn` フックのエクスポート (クライアントの React コンポーネントから server function を呼ぶ際のラッパー、redirect/error 連携用)。実装は 35 行。
2 行目：`@tanstack/start-client-core` を丸ごと再エクスポート (createServerFn / createMiddleware / createIsomorphicFn など)。

メッセージ：「`@tanstack/react-start` は start-client-core の React 用 adapter にすぎない」。
- React 固有のロジックは useServerFn の 35 行に閉じ込められている
- public API (createServerFn 等) は framework 非依存な core 側にある
- だから solid-start も同じ構造で薄く成立する (= framework adapter pattern)

Fact 03 (2 系統 + plugin) の主張を「Start 側のスタックは adapter pattern で構成されている」という新しい角度から閉じる。
LT で印象に残るスライドにしたい — 2 行 / 35 行 の数字を強調しつつ、最後で "adapter pattern" の meta-insight を投下。
-->

---
layout: section
class: 'bg-neutral-900 text-white'
---

<div class="absolute top-12 right-16 w-56 h-56 rounded-full bg-cyan-500/10"></div>
<div class="absolute bottom-16 left-20 w-40 h-40 rounded-lg bg-amber-500/10 rotate-12"></div>

<p class="text-sm font-semibold tracking-widest uppercase opacity-60 mb-4">Section 02 · Philosophy</p>

# <span class="text-7xl font-black tracking-tighter">Client-first</span>

<p class="text-xl font-medium mt-6 opacity-90 max-w-3xl">
  公式が <span class="inline-flex items-center gap-1.5 bg-red-500 text-white px-2.5 py-0.5 rounded font-bold tracking-widest text-sm align-middle"><span class="text-yellow-300">⚠</span> CRITICAL</span> と書いた設計思想
</p>

<!--
セクション 02。ここから「Client-first」が単なるマーケコピーではなく、公式が CRITICAL と明記している設計思想だと示す。
流れ：SKILL.md の引用 → 歴史的背景 (RSC 全盛と Tanner の批判) → コードでの opt-out 方法 (`ssr: false` / SPA mode / data-only)。
"Client-first を選べる仕組み" を順に展開していく。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Citation</p>

# <span class="text-4xl font-extrabold tracking-tight">公式 SKILL.md より直接引用</span>

<p class="text-neutral-600 mt-3"><code>packages/router-core/skills/router-core/SKILL.md</code> — TanStack チーム自身が AI agent 向けに書いたガイドライン。</p>

<div class="mt-8 p-8 bg-neutral-900 text-white rounded-lg">

<div class="text-xs font-semibold tracking-widest uppercase text-red-400 mb-3">⚠ CRITICAL</div>

<p class="text-xl font-bold leading-relaxed">
  TanStack Router is <span class="text-cyan-400">CLIENT-FIRST</span>. Loaders run on the <span class="text-cyan-400">client by default</span>, NOT server-only like Remix/Next.js.
</p>

<p class="text-sm opacity-60 mt-4 font-mono">— TanStack/router · router-core/SKILL.md</p>

</div>

<v-click>

<div class="grid grid-cols-3 gap-4 mt-6">
  <div class="bg-cyan-50 p-4 rounded-lg">
    <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">Router 単体</div>
    <div class="text-sm mt-1">client-first がデフォルト</div>
  </div>
  <div class="bg-amber-50 p-4 rounded-lg">
    <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">Start を被せると</div>
    <div class="text-sm mt-1">初期マッチは SSR がデフォルト</div>
  </div>
  <div class="bg-emerald-50 p-4 rounded-lg">
    <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">寄せ方</div>
    <div class="text-sm mt-1"><code>defaultSsr: false</code> / <code>ssr: false</code> / SPA mode</div>
  </div>
</div>

</v-click>

<!--
TanStack チーム自身が AI agent 向けに書いた `router-core/SKILL.md` からの直接引用。
"TanStack Router is CLIENT-FIRST. Loaders run on the client by default, NOT server-only like Remix/Next.js." ⚠ CRITICAL タグ付き。
注意したい階層：
- Router 単体 = client-first がデフォルト
- Start を被せると初期マッチは SSR がデフォルト (ハイブリッド方向)
- client に寄せたければ `defaultSsr: false` / per-route `ssr: false` / SPA mode で opt-out できる
"client-first を選べる" であって "強制" ではない、というニュアンスを丁寧に。
-->

---
layout: two-cols-header
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">History</p>

# <span class="text-4xl font-extrabold tracking-tight">Server-first 全盛 → Client-first 回帰</span>

::left::

<div class="bg-gray-100 p-3 rounded-lg font-mono text-xs leading-relaxed mt-2">

```text
2020  Next.js getServerSideProps
       ↓ SSR が「正解」に
2022  Remix loaders / actions
       ↓ Web 標準回帰
2023  Next.js App Router + RSC
       ↓ Server Components 強制化
2024  RSC が事実上の標準
       ↓
2025-09  Start v1 RC アナウンス
       ↓ feature-complete / API stable
2026-04  Start も RSC を experimental サポート
       ↓ isomorphic-first / Composite Components
2026-05  まだ RC (1.0 日付は未公表)
```

</div>

<p class="text-[10px] text-neutral-500 mt-1.5 leading-snug">
  ※ <code>@tanstack/react-start</code> の <span class="font-semibold">package version は 1.x</span> / 公式サイトの <span class="font-semibold">product status は RC</span> — 混同に注意
</p>

::right::

## <span class="text-lg font-bold">Tanner Linsley の主張</span>

<v-clicks>

<div class="bg-cyan-50 px-3 py-1.5 rounded-lg mt-1.5">
  <div class="text-[10px] font-semibold tracking-widest uppercase text-cyan-600">命名批判</div>
  <div class="text-sm font-bold text-neutral-800">"Server" ではなく <span class="text-cyan-700">"Pre-render" Components</span> と呼ぶべき</div>
  <div class="text-xs text-neutral-600 leading-snug mt-0.5">
    サーバー専用ではない — ビルド時 / Web Worker / Edge でも動く。<span class="font-medium">overreacted.io</span> はビルド時 RSC を CDN 静的配信
  </div>
</div>

<div class="bg-cyan-50 px-3 py-1.5 rounded-lg">
  <div class="text-[10px] font-semibold tracking-widest uppercase text-cyan-600">本質</div>
  <div class="text-sm font-bold text-neutral-800">RSC = <code class="text-xs">AsyncIterable&lt;string&gt;</code> のテキストストリーム</div>
  <div class="text-xs text-neutral-600 leading-snug mt-0.5">
    マジックなブラックボックスではなく<span class="font-semibold">データプリミティブ</span>。だから Query に流し込んで <code>staleTime</code> / 粒度の高い invalidation が効く
  </div>
</div>

<div class="bg-cyan-50 px-3 py-1.5 rounded-lg">
  <div class="text-[10px] font-semibold tracking-widest uppercase text-cyan-600">姿勢</div>
  <div class="text-sm font-bold text-neutral-800">SPA の 10 年を捨てるのではなく <span class="text-cyan-700">"磨く"</span></div>
  <div class="text-xs text-neutral-600 leading-snug mt-0.5">
    SSR + Hydration のメンタルモデルは依然強力。全部やり直すのは過剰、<span class="font-semibold">型安全性で進化</span>させて必要な時だけサーバーに委譲
  </div>
</div>

</v-clicks>

<!--
歴史的背景：2020 Next.js getServerSideProps → 2023 RSC でデフォルトが Server-first になった。
タイムライン補足：2025-09 Start v1 RC、2026-04 Start も RSC を experimental サポート、2026-05 時点でまだ RC (1.0 日付は未公表)。
混同注意：`@tanstack/react-start` の package version は 1.x だが、サイト上の product status は RC。

右側は Tanner Linsley の RSC に対するスタンス：
- 命名批判: RSC は本来 "Pre-render Components" / "Serializable Components" と呼ぶべき (ビルド時 / Web Worker / Edge でも動くため)
- 本質: RSC は `AsyncIterable<string>` という単純な primitive。Dan Abramov の overreacted.io はビルド時 RSC で Cloudflare CDN 配信
- 姿勢: SPA の 10 年の知見を "捨てて RSC でやり直す" のではなく "磨き上げる" 方向
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Selective SSR</p>

# <span class="text-4xl font-extrabold tracking-tight">SSR は "選ぶ" もの</span>

<p class="text-sm text-neutral-600 mt-2">ルートごとに <code>ssr</code> で実行場所を選ぶ。Start は SSR デフォルト、<code>ssr: false</code> なら loader から <code>localStorage</code> も直接触れる。</p>

```tsx {all|2-6|7-10|all}{lines:true}
// 3 つから選ぶ — 同じデッキ内で混在 OK
createFileRoute('/dashboard')({
  ssr: true,                  // フル SSR (デフォルト)
  // ssr: 'data-only',           // データだけ server
  // ssr: false,                 // 完全 CSR
})
createFileRoute('/canvas')({     // ssr: false → loader からブラウザ API
  ssr: false,
  loader: () => ({ saved: localStorage.getItem('canvas') }),
})
```

<div class="grid grid-cols-3 gap-3 mt-3">

<v-click>

<div class="bg-cyan-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">true</div>
  <div class="text-xs mt-1">loader + render 共に server</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">'data-only'</div>
  <div class="text-xs mt-1">loader は server、UI は client</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">false</div>
  <div class="text-xs mt-1">どちらも client (<code>localStorage</code> も loader から)</div>
</div>

</v-click>

</div>

<!--
ルート単位で SSR の実行場所を選ぶ (旧 Selective SSR + ssr: false 統合):
- `true` (デフォルト): loader と render 両方サーバー
- `'data-only'`: loader はサーバー、UI 描画はクライアント (次スライドで詳説)
- `false`: 完全クライアント — `localStorage` / `window` も loader から直接触れる

`'use client'` と違いルート単位で明示。同じデッキ内で SSR / CSR の混在も可能。
全体方針は `defaultSsr: false` でひっくり返せる。Next.js App Router の「デフォルト Server Component / `'use client'` で opt-out」とは方向が逆。
要点：「Router 単体は client-first / Start は SSR デフォルト / `defaultSsr: false` や SPA mode で client-first に寄せられる」を口頭で補足する。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Hybrid Mode</p>

# <span class="text-4xl font-extrabold tracking-tight"><code class="font-mono text-cyan-600">'data-only'</code> の使いどころ</span>

<p class="text-sm text-neutral-600 mt-2">
  data だけ server で取り、UI 描画は client に任せる中間モード。<span class="font-bold">SEO を維持しつつサーバー負荷を減らす</span>実用バランスが取れる。
</p>

<div class="grid grid-cols-3 gap-3 mt-4">

<div class="bg-cyan-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">ssr: true</div>
  <div class="text-xs text-neutral-500 mt-1">フル SSR</div>
  <ul class="text-xs text-neutral-700 mt-2 space-y-0.5">
    <li>SEO: ◎</li>
    <li>FCP: 良い</li>
    <li>Server 負荷: 高 (data + render)</li>
  </ul>
</div>

<div class="bg-amber-50 p-3 rounded-lg ring-2 ring-amber-400">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-700">'data-only'</div>
  <div class="text-xs text-neutral-500 mt-1">ハイブリッド</div>
  <ul class="text-xs text-neutral-700 mt-2 space-y-0.5">
    <li>SEO: ◎ (data 埋め込み)</li>
    <li>FCP: <span class="font-bold">非常に良い</span></li>
    <li>Server 負荷: <span class="font-bold">低 (data のみ)</span></li>
  </ul>
</div>

<div class="bg-emerald-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">ssr: false</div>
  <div class="text-xs text-neutral-500 mt-1">フル CSR</div>
  <ul class="text-xs text-neutral-700 mt-2 space-y-0.5">
    <li>SEO: △</li>
    <li>FCP: 遅め</li>
    <li>Server 負荷: 最小</li>
  </ul>
</div>

</div>

<v-click>

<div class="mt-4 p-3 bg-neutral-900 text-white rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase opacity-60">使い所</div>
  <div class="text-sm font-medium mt-0.5">
    複雑なコンポーネントツリーを持つ <span class="text-cyan-400">ダッシュボード / SaaS / 管理画面</span> に最適。React tree 全体を server で組み立てるコストを払わず、初期 data だけ先回りで渡せる。
  </div>
</div>

</v-click>

<!--
`ssr: 'data-only'` の使いどころ。データは server で取得して HTML に埋め込み、UI 描画は client に任せる。
利点：SEO は維持 (データが HTML に乗る) / FCP が非常に良い / Server 負荷は最小 (React tree を組み立てない)。
React tree が複雑なダッシュボード / SaaS / 管理画面で、サーバー側 render コストを抑えたいケースにハマる。
"フル SSR か CSR か" の二択ではなく、データだけ先回りという第 3 の選択肢があると説明する。
-->

---
layout: two-cols-header
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">SPA Mode</p>

# <span class="text-4xl font-extrabold tracking-tight">デッキ全体をクライアントへ</span>

::left::

## <span class="text-xl font-bold">vite.config.ts</span>

```ts {lines:true}
import { tanstackStart } from '@tanstack/react-start/plugin/vite'

export default defineConfig({
  plugins: [
    tanstackStart({
      spa: {
        enabled: true,
      },
    }),
  ],
})
```

::right::

## <span class="text-lg font-bold">SPA Mode の特徴</span>

<v-clicks>

<div class="bg-cyan-50 p-3 rounded-lg mt-2">
  <span class="font-semibold text-sm">初期 HTML はシェルだけ</span>
  <div class="text-xs text-neutral-600">レンダリングは全てクライアント</div>
</div>

<div class="bg-emerald-50 p-3 rounded-lg">
  <span class="font-semibold text-sm">サーバー機能は捨てない</span>
  <div class="text-xs text-neutral-600">Server Functions / Routes はそのまま</div>
</div>

<div class="bg-amber-50 p-3 rounded-lg">
  <span class="font-semibold text-sm">静的ホスティングに乗る</span>
  <div class="text-xs text-neutral-600">Cloudflare Pages / Netlify / S3</div>
</div>

<div class="p-3 bg-neutral-900 text-white rounded-lg">
  <div class="text-sm font-medium">"SPA mode does <b>not</b> mean giving up server-side features."</div>
  <div class="text-xs opacity-70 mt-0.5">→ SPA Mode = サーバー機能を捨てる、ではない</div>
</div>

</v-clicks>

<!--
デッキ全体を CSR 化する SPA Mode。`vite.config.ts` の plugin オプションで一行 (`spa: { enabled: true }`)。
誤解されやすい点：SPA Mode = "サーバー機能を諦める" ではない。Server Functions / Server Routes はそのまま使える。
初期 HTML はシェルのみ、レンダリングはクライアント。Cloudflare Pages / Netlify / S3 など静的ホスティングに乗せられる。
社内ツール / 管理画面で「サーバーは欲しいが SSR まではいらない」ケースに最適。Tanner が言う "段階的導入の困難さ" の解 のひとつ。
-->

---
layout: section
class: 'bg-amber-500 text-neutral-900'
---

<div class="absolute top-10 right-10 w-48 h-48 rounded-full bg-neutral-900/5"></div>
<div class="absolute bottom-10 left-10 w-32 h-32 rounded-lg bg-neutral-900/10 rotate-12"></div>

<p class="text-sm font-semibold tracking-widest uppercase opacity-70 mb-4">Section 03 · Robustness</p>

# <span class="text-7xl font-black tracking-tighter">堅牢性の源泉</span>

<p class="text-xl font-medium mt-6 opacity-80 max-w-3xl">
  ドーナツホール問題 / createServerFn / 型貫通
</p>

<!--
セクション 03。Client-first を選ぶことで何が "堅く" なるのか、8 スライドで示す。
中心トピック：(1) ドーナツホール問題の回避、(2) createServerFn という明示的 RPC、(3) 型の貫通、(4) URL as state、(5) パフォーマンス、(6) セキュリティ表面。
ここから LT のクライマックス。Zenn の論点 (RSC 批判 / `'use client'` の隠れたコスト / URL ステート管理) を集中投入。
-->

---
layout: two-cols-header
---

<p class="text-xs font-semibold tracking-widest uppercase text-amber-600 mb-2">Donut Hole</p>

# <span class="text-4xl font-extrabold tracking-tight">RSC が生む "認知のホール"</span>

::left::

```tsx {all|2-8|10-14|all}{lines:true}
// app/dashboard/page.tsx ─ Server Component
async function Dashboard() {
  const data = await db.query()      // ← server
  return (
    <ClientChart>                    {/* ← 'use client' */}
      <ServerInfo id={data.id} />    {/* ← 再び server (穴の中) */}
    </ClientChart>
  )
}
// ClientChart.tsx
'use client'
function ClientChart({ children }) {
  const [open, setOpen] = useState() // ← client (hook OK)
  return <button onClick={() => setOpen(!open)}>{children}</button>
}
```

<p class="text-xs text-neutral-500 mt-2 leading-snug">
  親 = server / 子 = client / 孫 = server。<span class="font-bold">どこで実行されているか</span>を毎行考えながら書く。
</p>

::right::

## <span class="text-xl font-bold">Next.js App Router の負担</span>

<v-clicks>

<div class="bg-amber-50 p-4 rounded-lg mt-3">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">境界の二重世界</div>
  <div class="text-sm mt-1"><code>'use client'</code> / <code>'use server'</code> を意識</div>
</div>

<div class="bg-amber-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">データコロケーション喪失</div>
  <div class="text-sm mt-1">親で取得 → 子へ props バケツリレー</div>
</div>

<div class="bg-amber-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">段階的導入の困難</div>
  <div class="text-sm mt-1">SPA からの移行は全面書き直し</div>
</div>

</v-clicks>

<v-click>

<p class="mt-4 text-sm text-neutral-700 italic">
  → Start は <span class="font-bold text-cyan-600">この穴自体を作らない</span>設計
</p>

</v-click>

<!--
RSC 採用フレームワークが抱える「ドーナツホール問題」。Tanner Linsley が PodRocket インタビューで挙げた "最大の課題"。
構造：Server Component の中に Client Component、その中にまた Server Component が children/slot として埋め込まれる。
開発者は「いま書いているこの行はサーバーか、クライアントか」を常に意識し続ける必要がある → 認知負荷が累積。

3 つの具体的負担：
- `'use client'` / `'use server'` の境界を常に意識
- インタラクティブ UI ではデータコロケーションが失われ、親に "巻き上げ" が必要 (Promise.all で props バケツリレー)
- SPA からの段階的導入が困難 — App Router は実質オールイン

Start はこの "穴" 自体を作らない設計 (= 次スライドの createServerFn パターン) を選ぶ。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Pattern · React Paris 2026</p>

# <span class="text-4xl font-extrabold tracking-tight">RSC も "ただの非同期データ"</span>

<p class="text-sm text-neutral-600 mt-2">サーバーで <code>renderToReadableStream(&lt;JSX/&gt;)</code>、クライアントで <code>useQuery</code> で受けるだけ。<span class="font-bold">境界はインポートのみ</span>、コードに "魔法" は出てこない。<span class="text-xs text-neutral-400">2026-04 React Paris / BeJS で実演</span></p>

```tsx {all|2-7|9-13|all}{lines:true}
// server: renderToReadableStream で JSX → RSC payload (react-server-dom 系)
const renderPost = createServerFn({ method: 'GET' })
  .inputValidator((id: string) => id)
  .handler(async ({ data: id }) => {
    const post = await db.post.findUnique({ where: { id } })
    return renderToReadableStream(<PostView post={post} />)
  })
// client: useQuery で受けて、そのまま描画 (デシリアライズはフレームワーク)
const { data: postJsx } = useQuery({
  queryKey: ['post', postId],
  queryFn: () => renderPost({ data: postId }),
})
return <>{postJsx}</>
```

<div class="grid grid-cols-3 gap-3 mt-3">

<v-click>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">JSX = data</div>
  <div class="text-xs mt-0.5">RSC を Query で扱う</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">Validation</div>
  <div class="text-xs mt-0.5"><code>.inputValidator()</code> で型検証</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">Cache</div>
  <div class="text-xs mt-0.5"><code>staleTime</code> / <code>gcTime</code> 直接適用</div>
</div>

</v-click>

</div>

<!--
2026-04 BeJS / React Paris で Tanner Linsley が実演した正式 API。
サーバー側で `renderToReadableStream(<JSX/>)` → クライアント側で `useQuery` の queryFn として呼ぶだけ。
ストリームのデシリアライズはフレームワークが吸収するので、コードに "魔法" や暗黙の境界は現れない。

Tanner の哲学："RSC は サーバー状態にすぎない" → だったら TanStack Query で扱えばいい：
- staleTime / gcTime / refetchInterval などの既存メカニズムがそのまま効く
- queryKey 単位の選択的 invalidation
- Next.js のような予測不可能な「コンポーネント専用キャッシュ層」は不要

データを返せば普通の Server Function、JSX を返せば RSC ストリーム。同じ formula で両方扱える点を強調する。
基本的な data-only `createServerFn` パターンは Slide 20 (Streaming) でも同じ手触りで登場するので、ここでは "RSC も使えるよ" を見せる。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">2026 · Inversion of Control</p>

# <span class="text-4xl font-extrabold tracking-tight">Composite Components</span>

<p class="text-sm text-neutral-600 mt-2">サーバーは <span class="font-bold">枠組み + データ</span>だけ提供、UI の組み立てはクライアントから注入。<span class="text-xs text-neutral-400">2026-03 動画 "Without the Mental Gymnastics" / 2026-04 BeJS で実演された次の一手</span></p>

```tsx {all|2-7|9-15|all}{lines:true}
// server: スロットで "穴" を空ける
return createCompositeComponent((props) => (
  <div>
    <h2>CPU Cores: {cpuCount}</h2>
    {props.children}                       /* ← クライアントが注入 */
    {props.footer({ cpuCount })}           /* ← データ付きスロット */
  </div>
))
// client: スロットに自由にコンポーネントを差し込む
<CompositeComponent
  src={data}
  footer={({ cpuCount }) => <StatusBar cores={cpuCount} />}
>
  <UserSettings />
</CompositeComponent>
```

<div class="grid grid-cols-3 gap-3 mt-3">

<v-click>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">Cache 独立</div>
  <div class="text-xs mt-0.5">枠組みは長期キャッシュ / スロットだけ動的差替</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">SPA mental model</div>
  <div class="text-xs mt-0.5">組み立ての主導権はクライアント</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">no <code>'use client'</code></div>
  <div class="text-xs mt-0.5">大多数のケースでディレクティブ不要に</div>
</div>

</v-click>

</div>

<!--
Tanner Linsley が 2026-03 動画 "Server Components Without the Mental Gymnastics" と 2026-04 BeJS で実演した次世代パターン。
キーワードは Inversion of Control (IoC = 制御の反転)：

- 通常の RSC (Next.js): サーバーが「ここで ClientCounter を使う」と決め打ちで子コンポーネントを import
- Composite Components: サーバーは "枠組み (スロット位置) + データ" だけ提供 → 中身はクライアントから差し込む
  (React の Render Props / Function as Child を Network Boundary で実現するイメージ)

3 つの利点：
- Cache 独立: スロットの状態変化でサーバー枠組み全体を revalidate しない。サーバー由来のマークアップは CDN / TanStack Query に長期キャッシュ、スロットだけ動的差替で済む
- SPA メンタルモデル: UI の組み立ては引き続きクライアントが主導。Tanner の "SPA を磨く" 哲学の具体形
- 'use client' 不要: インタラクティブな UI を作るたびに `'use client'` でファイルを切る必要がなくなる

LT で深掘りすると重いので、「Start は単に RSC をサポートするのではなく、根本から再設計している」というメッセージで通過。質問が来たら詳細を語る。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Streaming · AI</p>

# <span class="text-4xl font-extrabold tracking-tight">Server Function はストリーミングできる</span>

<p class="text-sm text-neutral-600 mt-2">
  <code>async function*</code> を返すだけで<span class="font-bold">型付きストリーム</span>に。AI/LLM の trickle UI と最高に相性が良い。公式 docs も「AI アプリの台頭で特に人気」と明記。
</p>

```ts {all|2-5|3|7-13|all}{lines:true}
// server: TanStack AI を Server Function で包む
const streamChat = createServerFn({ method: 'POST' })
  .inputValidator(ChatInputSchema)
  .handler(async function* ({ data }) {
    const stream = chat({                       // ← @tanstack/ai
      adapter: openaiText('gpt-4o'),
      messages: data.messages,
      tools: [getProducts, searchProducts],     // ← tool 呼び出しもサーバー側で安全に
    })
    for await (const chunk of stream) {
      yield chunk                               // ← chunk の型がクライアントまで貫通
    }
  })
```

<div class="grid grid-cols-4 gap-2 mt-3">

<v-click>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">Typed Stream</div>
  <div class="text-xs mt-0.5">yield の型がクライアントへ</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">API Key 隔離</div>
  <div class="text-xs mt-0.5">サーバー閉じ込め、漏洩しない</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">Tools 対応</div>
  <div class="text-xs mt-0.5">関数呼び出しもサーバー側</div>
</div>

</v-click>

<v-click>

<div class="bg-purple-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-purple-600">No SSE 配線</div>
  <div class="text-xs mt-0.5"><code>for await</code> で消費するだけ</div>
</div>

</v-click>

</div>

<!--
Server Function は `async function*` (async generator) を返せる → 型付きストリーミングが成立。
公式 docs も「AI アプリの台頭で特に人気」と明記しているパターン。LLM の trickle UI / chat / 検索の段階表示と相性 ◎。
4 つの利点：
- yield の型が end-to-end で貫通
- API キーはサーバーに閉じ込め
- tool 呼び出しもサーバー側で安全
- SSE / WebSocket の手配線は不要、`for await` で消費するだけ
RSC や Server Actions を経由せず、TanStack Query + Server Function だけでストリーミング UI が組める。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Server Routes</p>

# <span class="text-4xl font-extrabold tracking-tight">REST API も同じファイルで</span>

<p class="text-sm text-neutral-600 mt-2">Next.js の Route Handlers と同じ立て方。さらに <span class="font-bold">page route と同居</span>できる。</p>

```ts {all|3-4|5-9|all}{lines:true}
// routes/posts.$id.ts
export const Route = createFileRoute('/posts/$id')({
  component: PostPage,                              // ← UI (GET text/html)
  loader: ({ params }) => fetchPost(params.id),
  server: {                                         // ← REST API も同居
    handlers: {
      GET: async ({ params }) => Response.json(await db.post.find(params.id)),
      DELETE: async ({ params }) => { /* … */ },
    },
  },
})
```

<div class="grid grid-cols-3 gap-3 mt-3">

<v-click>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">Web 標準</div>
  <div class="text-xs mt-0.5"><code>Request</code> / <code>Response</code> のみ</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">Page と同居</div>
  <div class="text-xs mt-0.5">Next.js は別ファイル必須</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">3つのサーバー処理</div>
  <div class="text-xs mt-0.5">loader / serverFn / handlers</div>
</div>

</v-click>

</div>

<!--
REST API も同じ `routes/` ディレクトリで定義できる。Next.js Route Handlers と立て方は同じだが、page route と "同居" できるのが差分。
1 つの `routes/posts.$id.ts` に component (UI) / loader / server.handlers (GET/POST/DELETE) を全部書ける → コロケーションが効く。
Web 標準 (`Request` / `Response`) のみで、Node 固有 API に依存しない → Edge / Workers / Bun / Deno にそのまま乗る。
使い分け：UI 起点なら loader / 関数呼び出しなら serverFn / 外部からの HTTP なら handlers。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Type Safety</p>

# <span class="text-4xl font-extrabold tracking-tight">型がクライアントから貫通する</span>

```ts {all|2-5|8-13|all}{lines:true}
// ルート定義
export const Route = createFileRoute('/posts/$postId')({
  loader: ({ params }) => fetchPost(params.postId),
  validateSearch: (s) => ({ tab: s.tab as 'comments' | 'likes' }),
})

// 呼び出し側 — 全部 type-safe
<Link
  to="/posts/$postId"
  params={{ postId: '123' }}
  search={{ tab: 'comments' }}   // 'comments' | 'likes' 以外は型エラー
/>
```

<div class="grid grid-cols-2 gap-4 mt-6">

<v-click>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">Compile-time</div>
  <div class="text-sm mt-1"><code>params</code> / <code>search</code> / <code>loader</code> 戻り値が全て決まる</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">公式 SKILL.md</div>
  <div class="text-sm mt-1">"TanStack Router types are <span class="font-bold">FULLY INFERRED</span>. Never cast."</div>
</div>

</v-click>

</div>

<v-click>

<p class="mt-5 text-sm text-neutral-700">
  Client-first だから可能 — ルートツリーが静的に解析でき、クライアント側 TypeScript で全推論される。
</p>

</v-click>

<!--
型がクライアントから貫通する。ルート定義の `params` / `validateSearch` / `loader` の戻り値が、`<Link>` / `useNavigate` / `useSearch` まで完全に推論される。
`search={{ tab: 'invalid' }}` のように許可されない値を書くとコンパイルエラー。
SKILL.md 引用：「TanStack Router types are FULLY INFERRED. Never cast.」型キャスト禁止の文化。
Client-first だから可能 — ルートツリーが静的解析でき、クライアント側 TypeScript で全推論される。サーバー側コンパイルに依存する RSC とは技術的に別の土俵。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">URL as State</p>

# <span class="text-4xl font-extrabold tracking-tight">クエリパラメータが "本物のState" になる</span>

<p class="text-sm text-neutral-600 mt-1">Zod スキーマ + middleware で、URL を <span class="font-bold">型安全 / 検証付き / 共有可能</span>な状態管理にする。<span class="text-xs text-neutral-400">※ Zod v4 ならスキーマを直接 validateSearch に渡してもよい</span></p>

```ts {all|2-6|8-9|10|11|all}{lines:true}
// Zod スキーマで URL の形を定義
const SearchSchema = z.object({
  page: z.number().int().positive().catch(1),
  tab: z.enum(['comments', 'likes']).catch('comments'),
  theme: z.enum(['light', 'dark']).catch('light'),
})
export const Route = createFileRoute('/posts')({
  validateSearch: zodValidator(SearchSchema),  // ← 型 + ランタイム検証
  search: { middlewares: [
    stripSearchParams({ page: 1 }),            // ← デフォ値は URL から消す
    retainSearchParams(['theme']),             // ← theme は遷移しても保持
  ]},
})
```

<div class="grid grid-cols-4 gap-2 mt-2">

<v-click>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">validateSearch</div>
  <div class="text-xs mt-0.5">不正な URL は <code>.catch()</code> で復旧</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">strip</div>
  <div class="text-xs mt-0.5">デフォルト値を URL から除去</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">retain</div>
  <div class="text-xs mt-0.5">ルート横断で保持</div>
</div>

</v-click>

<v-click>

<div class="bg-purple-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-purple-600">structuralSharing</div>
  <div class="text-xs mt-0.5"><code>select</code> で再描画最適化</div>
</div>

</v-click>

</div>

<!--
URL クエリパラメータが "本物の状態管理" になる、というスライド。Zenn 記事 "TanStack Routerで実現する高度なクエリパラメータ管理" の核心。
Tanner の主張："URL は最古のステート管理 — 高速・共有可能・直感的" / "バリデーション・型・所有権をルーターに持たせれば URL は文字列ではなく真の状態になる"。

コードで示す 4 つの武器：
- validateSearch + Zod で型 + ランタイム検証。`.catch()` で不正値からも復旧
- stripSearchParams でデフォ値を URL から削除 → URL がクリーンに保たれる (`?page=1` のような無駄を消す)
- retainSearchParams で theme / lang など横断パラメータをルート遷移しても保持
- useSearch({ select, structuralSharing }) で必要なパラメータだけ subscribe → 不要な再レンダリングを抑制
JSON-first なので入れ子オブジェクトや配列も自然に扱える。検索 / フィルタ画面で威力。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Performance</p>

# <span class="text-4xl font-extrabold tracking-tight">Client-first でも SSR は遅くない</span>

<div class="grid grid-cols-2 gap-6 mt-6">

<div class="bg-cyan-50 p-5 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">SSR Benchmark</div>

  <div class="text-sm text-neutral-700 leading-relaxed">
    Platformatic の corrected benchmark では、TanStack Start は 1,000 req/s を 100% success で処理。
  </div>

  <div class="mt-3 text-4xl font-black text-cyan-500">18ms</div>
  <div class="text-xs text-neutral-500 mt-1">average latency / corrected result</div>

  <div class="text-xs text-neutral-600 mt-3 leading-relaxed">
    React Router も 19ms 程度で近く、Next.js は同条件で苦戦。
  </div>
</div>

<div class="bg-neutral-900 text-white p-5 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-400 mb-2">Vite 8 + Rolldown</div>

  <div class="text-sm leading-relaxed opacity-90">
    Vite 8 は Rolldown を標準 bundler に採用。Rolldown 1.0 stable は 2026-05-07 リリース。
  </div>

  <div class="mt-3 text-4xl font-black text-amber-400">10–30x</div>
  <div class="text-xs opacity-60 mt-1">faster builds と公式が説明</div>

  <div class="text-xs opacity-70 mt-3 leading-relaxed">
    Start は Vite plugin として構成されるため、Vite の toolchain 進化を受けやすい。
  </div>
</div>

</div>

<v-click>

<div class="mt-4 p-3 bg-gray-100 rounded-lg">
  <div class="text-sm font-bold">
    takeaway: 「client-first」は SSR を捨てる設計ではない。
  </div>
  <div class="text-xs text-neutral-600 mt-0.5">
    必要な場所では SSR も速く、build pipeline は Vite / Rolldown の進化に乗れる。
  </div>
</div>

</v-click>

<!--
「Client-first は SSR を諦めた設計ではない」という反論スライド。
Platformatic の corrected benchmark で TanStack Start は 1,000 req/s を 100% 成功、平均 18ms。
React Router (19ms) と同等、Next.js は同条件で苦戦。
ビルド側：Vite 8 が Rolldown を標準採用、Rolldown 1.0 stable は 2026-05-07 リリース。公式説明で 10–30x faster builds。
Start は Vite plugin なので toolchain の進化に直接乗れる → Tanner Linsley の Server Functions (GET) なら HTTP キャッシュも効くという PodRocket の主張も合わせて補足できる。
「クライアントファースト = 妥協」という誤解を数字で消すスライド。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Security Surface</p>

# <span class="text-4xl font-extrabold tracking-tight">RSC-first のコスト</span>

<p class="text-sm text-neutral-600 mt-2">
  2025年末以降、React Server Components / Next.js App Router 周辺で重大な advisory が連続。複雑な protocol / cache / 境界は <span class="font-bold">attack surface</span> でもある。
</p>

<div class="grid grid-cols-3 gap-3 mt-4">

<div class="bg-neutral-900 text-white p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-red-400 mb-1">2025-12</div>
  <div class="text-sm font-bold">React2Shell</div>
  <div class="text-xs opacity-70 mt-1">CVE-2025-55182 / CVSS 10.0</div>
  <div class="text-xs opacity-70">Next.js advisory: GHSA-9qr9-h5gf-34mp</div>
  <div class="text-xs opacity-60 mt-1">RSC payload の unsafe deserialization による unauthenticated RCE</div>
</div>

<div class="bg-amber-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600 mb-1">2025-12 follow-up</div>
  <div class="text-sm font-bold text-neutral-900">RSC fixes</div>
  <div class="text-xs text-neutral-600 mt-1">DoS / source code exposure</div>
  <div class="text-xs text-neutral-500 mt-1">Patched: 15.5.16 / 16.2.5+</div>
</div>

<div class="bg-cyan-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-1">2026-05</div>
  <div class="text-sm font-bold text-neutral-900">Coordinated release</div>
  <div class="text-xs text-neutral-600 mt-1">Server / Cache Components DoS</div>
  <div class="text-xs text-neutral-600">RSC cache poisoning</div>
  <div class="text-xs text-neutral-600">Middleware / Proxy bypass</div>
</div>

</div>

<v-click>

<div class="mt-4 p-3 bg-gray-100 rounded-lg">
  <div class="text-sm font-bold">RSC は強力。ただし、Flight protocol / cache / server-client 境界が複雑。</div>
  <div class="text-xs text-neutral-600 mt-0.5">
    Start は <span class="font-semibold">RSC-first ではなく</span>、<code>createServerFn</code> / <code>loader</code> / <code>server.handlers</code> を明示的に切る設計を選べる (<code>'use server'</code> は<span class="font-semibold">意図的に非対応</span>)。RSC 利用時は React upstream の影響を受け得る点には注意。
  </div>
</div>

</v-click>

<!--
2025 年末以降、RSC / Next.js 周辺の脆弱性が立て続けに出ている事実をまとめる。
- 2025-12 React2Shell (CVE-2025-55182, CVSS 10.0): RSC payload の unsafe deserialization による unauthenticated RCE。Next.js advisory GHSA-9qr9-h5gf-34mp
- 2025-12 follow-up: DoS / source code exposure。15.5.16 / 16.2.5 で修正
- 2026-05 coordinated release: Server/Cache Components DoS、RSC cache poisoning、Middleware/Proxy bypass

論点：RSC は強力。ただし Flight protocol / cache / server-client 境界が複雑 → attack surface が広い。
Tanner の主張：Flight プロトコルは "双方向" であることが攻撃面を広げている。Start は単方向 (サーバー → クライアントのみ) + 入力検証必須の RPC という設計を取る。
"FUD ではなく事実" として淡々と扱う。RSC を否定するのではなく "コストがあることを認識して選ぶ" のが LT の趣旨。
-->

---
layout: two-cols-header
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Design Choice</p>

# <span class="text-4xl font-extrabold tracking-tight"><code class="font-mono text-cyan-600">use server</code> が悪い、ではない</span>

<p class="text-sm text-neutral-600 mt-2">
  React2Shell の根本原因は <span class="font-bold">RSC / Flight protocol の unsafe deserialization</span>。<code>'use server'</code> / Server Actions は、その問題が外部入力と結びつく<span class="font-bold">代表的な入口</span>になった。
</p>

::left::

## <span class="text-base font-bold text-neutral-700">代表的な入口になった理由</span>

<div class="space-y-2 mt-2">

<div class="bg-amber-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">暗黙の境界</div>
  <div class="text-xs mt-0.5"><code>'use server'</code> を書くだけで network boundary が生まれる</div>
</div>

<div class="bg-amber-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">引数の経路</div>
  <div class="text-xs mt-0.5">引数は client から serialize されて届く (untrusted input)</div>
</div>

<div class="bg-amber-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">decoder 依存</div>
  <div class="text-xs mt-0.5">framework 側の protocol / decoder の実装に safety を預ける</div>
</div>

</div>

::right::

## <span class="text-base font-bold text-cyan-600">TanStack Start の選択</span>

<div class="space-y-2 mt-2">

<v-clicks>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">非対応</div>
  <div class="text-xs mt-0.5"><code>'use server' actions</code> は<span class="font-semibold">意図的にサポートしない</span></div>
</div>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">明示的 RPC</div>
  <div class="text-xs mt-0.5"><code>createServerFn</code> で boundary をコードで宣言する</div>
</div>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">入力検証</div>
  <div class="text-xs mt-0.5">入力は untrusted として <code>.inputValidator()</code> 通過必須</div>
</div>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">RSC は opt-in</div>
  <div class="text-xs mt-0.5">client は Flight data を parse しない設計</div>
</div>

</v-clicks>

</div>

<p class="mt-3 text-xs text-neutral-500 leading-snug">
  ※ <code>createServerFn</code> も HTTP 越しの API surface。認証 / 入力検証 / 依存更新は引き続き必要。
</p>

<!--
誤解防止スライド。「`'use server'` が悪い」という話ではない。
React2Shell の根本原因は RSC / Flight protocol の unsafe deserialization。`'use server'` Server Actions は、その問題が外部入力と結びつく "代表的な入口" になった。

左：代表的入口になった構造的理由
- 暗黙の network boundary (文字列を書くだけで境界が生まれる)
- 引数が client から serialize されて届く (untrusted input)
- decoder の safety を framework に預ける

右：Start の選択
- `'use server'` actions は意図的に非対応
- 境界はコードで明示 (`createServerFn`)
- 入力は untrusted として `.inputValidator()` 必須
- RSC は opt-in

注釈：`createServerFn` も HTTP 越しの API surface であることに変わりはない。"安全になる" のではなく "boundary が見える" のが利点。
Zenn 記事 "use client は JavaScript 標準ではない" の論点 (出所不明な文字列、ロックイン、エコシステム断片化) もここで触れられる。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">More</p>

# <span class="text-4xl font-extrabold tracking-tight">他にもある "Start ならでは"</span>

<p class="text-sm text-neutral-600 mt-2">Next.js / Remix と差がつく細部の API たち。</p>

<div class="grid grid-cols-3 gap-3 mt-4">

<div class="bg-cyan-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-1">Platform</div>
  <div class="text-base font-bold text-neutral-900">デプロイ先非依存</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug">Vite plugin として動く。Node / Bun / Deno / Workers / Lambda にそのまま乗る。<span class="font-semibold">Vercel 結合なし</span>。</div>
</div>

<div class="bg-emerald-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 mb-1">Web Standards</div>
  <div class="text-base font-bold text-neutral-900"><code>Request</code> / <code>Response</code> 一次市民</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug">Node の <code>req</code>/<code>res</code> ではなく Web Fetch primitives。Edge / Workers に自然に乗る。</div>
</div>

<div class="bg-amber-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600 mb-1">Speed</div>
  <div class="text-base font-bold text-neutral-900">Intent Preloading</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug"><code>defaultPreload: 'intent'</code> でホバー時に次ルートの loader を先回り。デフォで体感が速い。</div>
</div>

<div class="bg-purple-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-purple-600 mb-1">Middleware</div>
  <div class="text-base font-bold text-neutral-900">型付き middleware chain</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug"><code>createMiddleware</code> + <code>beforeLoad</code> で context を型安全に伝播。<code>throw redirect()</code> で認可ガード。</div>
</div>

<div class="bg-rose-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-rose-600 mb-1">UX details</div>
  <div class="text-base font-bold text-neutral-900">Pending / Error をルート単位で</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug"><code>pendingMs</code> / <code>pendingMinMs</code> でローディング点滅まで防止。<code>errorComponent</code> も per-route。</div>
</div>

<div class="bg-neutral-900 text-white p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-400 mb-1">Qwik-flavored</div>
  <div class="text-base font-bold text-white">Deferred Hydration</div>
  <div class="text-xs text-neutral-300 mt-1 leading-snug"><code>&lt;Hydrate when={visible()}&gt;</code> — 条件が来るまで JS チャンクすら DL しない。</div>
</div>

</div>

<v-click>

<p class="mt-3 text-sm text-neutral-700">
  → どれも <span class="font-bold">「React で欲しかったが欠けていた細部」</span> をフレームワーク側で API 化したもの。
</p>

</v-click>

<!--
ここまでで主要論点は出し切ったので、最後に "Start ならではの細部" を 6 枚カードでサラッと紹介。
- Platform: Vite plugin、Node / Bun / Deno / Workers / Lambda に乗る、Vercel 結合なし
- Web Standards: Request / Response が一次市民、Edge / Workers 自然
- Speed: `defaultPreload: 'intent'` でホバー時 prefetch、デフォで体感速い
- Middleware: `createMiddleware` + `beforeLoad` で型付き context 伝播、`throw redirect()` で認可ガード
- UX: `pendingMs` / `pendingMinMs` でローディング点滅防止、`errorComponent` も per-route
- Qwik-flavored: `<Hydrate when={...}>` で条件付き hydration、JS チャンクの DL すら遅延
共通点："React で欲しかったが欠けていた細部" をフレームワーク側で API 化している。
時間が押していたらこのスライドはざっと舐めるだけで OK。
-->

---
layout: section
class: 'bg-purple-500 text-white'
---

<div class="absolute top-12 right-16 w-56 h-56 rounded-full bg-white/5"></div>

<p class="text-sm font-semibold tracking-widest uppercase opacity-70 mb-4">Section 04 · When</p>

# <span class="text-7xl font-black tracking-tighter">いつ選ぶか</span>

<p class="text-xl font-medium mt-6 opacity-90 max-w-3xl">
  Next.js / React Router (Remix) との使い分け
</p>

<!--
セクション 04。最終章。Start vs Next.js / Remix の使い分け。
「Start が万能」とは言わない。プロジェクトの性質で最適なツールは変わる、というフェアな姿勢で締める。
Tanner 本人も「RSC は静的コンテンツに有効、万能ではない」と発言している。
-->

---
layout: two-cols-header
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">When to pick</p>

# <span class="text-4xl font-extrabold tracking-tight">使い分け早見表</span>

::left::

<div class="bg-cyan-500 text-white p-6 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase opacity-80 mb-3">TanStack Start</div>
  <div class="text-xl font-bold mb-4">こんな時に刺さる</div>

  <ul class="space-y-2 text-sm">
    <li class="flex gap-2"><span class="font-bold">·</span> ダッシュボード / 管理画面</li>
    <li class="flex gap-2"><span class="font-bold">·</span> SaaS の認証後の体験</li>
    <li class="flex gap-2"><span class="font-bold">·</span> 社内ツール（静的ホスティング可）</li>
    <li class="flex gap-2"><span class="font-bold">·</span> 既存 SPA を壊さず移行したい</li>
    <li class="flex gap-2"><span class="font-bold">·</span> Type-safe routing 最優先</li>
  </ul>
</div>

::right::

<div class="bg-neutral-900 text-white p-6 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase opacity-60 mb-3">Next.js / Remix</div>
  <div class="text-xl font-bold mb-4">こんな時に刺さる</div>

  <ul class="space-y-2 text-sm">
    <li class="flex gap-2"><span class="font-bold">·</span> マーケサイト / メディア</li>
    <li class="flex gap-2"><span class="font-bold">·</span> e-commerce（ISR / streaming）</li>
    <li class="flex gap-2"><span class="font-bold">·</span> 大量のサーバー側データ集約</li>
    <li class="flex gap-2"><span class="font-bold">·</span> Vercel / RSC エコシステム</li>
    <li class="flex gap-2"><span class="font-bold">·</span> 採用市場の安心感</li>
  </ul>
</div>

<!--
左 (Start が刺さる)：ダッシュボード / 管理画面 / SaaS 認証後 / 社内ツール / 既存 SPA を壊さず移行したい / Type-safe routing 最優先。
右 (Next.js / Remix が刺さる)：マーケサイト / メディア / e-commerce (ISR / streaming) / 大量のサーバー側データ集約 / Vercel + RSC エコシステム / 採用市場の安心感。
ポイント：ツール選択はアプリケーションの特性次第。"Start ファン" として登壇しているが、宗教戦争にしない。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Wrap up</p>

# <span class="text-4xl font-extrabold tracking-tight">まとめ</span>

<div class="grid grid-cols-2 gap-4 mt-5">

<v-clicks>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-2xl font-black text-cyan-500 mb-1">01</div>
  <div class="text-base font-bold">Start ≒ Router の薄い層</div>
  <div class="text-xs text-neutral-600 mt-0.5">src 比較で 1.4%。独立リポジトリは無い</div>
</div>

<div class="bg-emerald-50 p-4 rounded-lg">
  <div class="text-2xl font-black text-emerald-500 mb-1">02</div>
  <div class="text-base font-bold">Client-first は公式設計</div>
  <div class="text-xs text-neutral-600 mt-0.5">SKILL.md が CRITICAL と明記</div>
</div>

<div class="bg-amber-50 p-4 rounded-lg">
  <div class="text-2xl font-black text-amber-500 mb-1">03</div>
  <div class="text-base font-bold">ドーナツホールを作らない</div>
  <div class="text-xs text-neutral-600 mt-0.5">境界はインポートだけ、認知負荷が低い</div>
</div>

<div class="bg-purple-50 p-4 rounded-lg">
  <div class="text-2xl font-black text-purple-500 mb-1">04</div>
  <div class="text-base font-bold">型が貫通する</div>
  <div class="text-xs text-neutral-600 mt-0.5">routing → loader → params → search</div>
</div>

</v-clicks>

</div>

<v-click>

<div class="mt-4 p-3 bg-neutral-900 text-white rounded-lg">
  <span class="text-xs font-semibold tracking-widest uppercase opacity-60">Takeaway</span>
  <div class="text-base font-bold mt-0.5">RSC が全てではない。<span class="text-cyan-400">クライアントファースト</span>を "選べる" 時代に。</div>
</div>

</v-click>

<!--
4 つの take away を再確認：
01 Start ≒ Router の薄い層 (src 比較で 1.4%、独立リポジトリは存在しない)
02 Client-first は公式設計 (SKILL.md に CRITICAL と明記)
03 ドーナツホールを作らない (境界はインポートだけ、認知負荷が低い)
04 型が貫通する (routing → loader → params → search、FULLY INFERRED)

最後のメッセージ：「RSC が全てではない。クライアントファーストを "選べる" 時代に」。
RSC を否定するのではなく、もう一つの選択肢を提示する LT として締める。
質疑応答用にチャットに流すリンク：
- TanStack Router (https://tanstack.com/router)
- TanStack Start (https://tanstack.com/start)
- Zenn 記事 3 本 (use client / TanStack Start RSC / Search params 管理)
ご清聴ありがとうございました。
-->

