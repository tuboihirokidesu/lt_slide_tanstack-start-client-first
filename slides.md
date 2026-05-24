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
    <div class="text-xl font-black text-emerald-500 mt-0.5">46 <span class="text-xs font-bold">files</span> · 187<span class="text-xs">KB</span></div>
  </div>
  <div class="bg-cyan-50 p-2 rounded-lg">
    <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">react-start/src</div>
    <div class="text-xl font-black text-cyan-500 mt-0.5">17 <span class="text-xs font-bold">files</span> · 3.8<span class="text-xs">KB</span></div>
  </div>
  <div class="bg-neutral-900 text-white p-2 rounded-lg">
    <div class="text-xs font-semibold tracking-widest uppercase opacity-60">逆算</div>
    <div class="text-xl font-black text-amber-400 mt-0.5">Router ≒ Start の <span class="text-cyan-400">49 倍</span></div>
  </div>
</div>

<p class="mt-2 text-sm text-neutral-700">
  → src 実測でも Start は Router の <span class="font-bold text-cyan-600">2.0%</span>。<span class="font-bold">"Start の 90% は Router"</span> は誇張ではなく実測。
</p>

</v-click>

<!--
1 つ目の事実。`github.com/TanStack/start` は実在しない (404)。Start の開発は `TanStack/router` リポジトリ内で進む。
左：404 のキャプチャ / 右：`TanStack/router` の description にすでに "client-first" / "server-capable" / "full-stack framework" と書かれている。
"Start の 90% は Router" は感覚値ではなく実測 — `react-router/src` (46 files, 187KB) vs `react-start/src` (17 files, 3.8KB) で 49 倍差。
※ RSC ファイル群 (rsc.tsx / rsc.rsc.ts / server.rsc.ts / hydration.ts 等) の追加で Start 側が肥えた結果、半年前の "73 倍" から縮小。
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
    <li>· Built-in caching / prefetching <span class="text-neutral-400">／ <span class="font-semibold">loader 結果</span>のキャッシュ + hover prefetch (Query と併用可)</span></li>
    <li>· Nested layouts + per-route pending/error <span class="text-neutral-400">／ 入れ子レイアウト + <code>pendingMs</code> でチラつき防止 (Next にはない delay 制御)</span></li>
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

<div class="mt-2 p-2.5 bg-neutral-900 text-white rounded-lg">
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

<p class="text-sm text-neutral-600 mt-2">ユーザーは <span class="font-bold">Start 系</span> と <span class="font-bold">Router 系</span> の 2 つを install する。Vite plugin が両者を繋いで 1 つのフルスタック framework になる。ランタイム (<code>react-start</code>) の外部依存は <code>pathe</code> のみ。</p>

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

外部依存については丁寧に: `@tanstack/react-start` (ランタイムエントリ) は外部依存 `pathe` 1 つだけ。
`start-client-core` も外部は `seroval` 1 つ (型付き serialization で createServerFn の RPC を実現)。
ただし `start-plugin-core` (build/dev 用 Vite plugin) は Babel / Rolldown plugin utils / cheerio / lightningcss / srvx / zod など 16 個の外部依存を持つ — これは build-time の話。
つまり: ランタイム = 極小 / build-time = 普通の Vite plugin、というのが正確。
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

# <span class="text-4xl font-extrabold tracking-tight"><code>react-start</code> の "実コード" は <span class="text-cyan-600">30 行</span></span>

<p class="text-sm text-neutral-600 mt-2"><code>react-start/src/index.ts</code> は <span class="font-bold">ほぼ全部 re-export</span> — 実装らしい実装は <code>useServerFn</code> という React フック <span class="font-bold">1 つ・30 行</span> に集約。</p>

```ts {all|1-2|4|all}{lines:true}
export { useServerFn } from './useServerFn'   // ← 唯一の "実コード"
export * from '@tanstack/start-client-core'   // ← core 丸ごと

// ※ 厳密には、この下に Vite SSR cycle 回避用の明示 re-export が数行続く (2026-05-21 追加)
```

<div class="grid grid-cols-2 gap-4 mt-4">

<v-click>

<div class="bg-cyan-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-1">唯一の "実コード"</div>
  <div class="text-xs text-neutral-700"><code>useServerFn</code> フック <span class="font-bold">30 行</span> — redirect / error を Router に連携するだけ。</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-3 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 mb-1">残り全部 = re-export</div>
  <div class="text-xs text-neutral-700">2 日前 (<span class="font-bold">2026-05-21</span>) に増えた行は <span class="font-bold">Vite のバグ workaround</span>。Start 側のロジック追加ではない。</div>
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
決定的証拠。`@tanstack/react-start` の "実コード" は useServerFn フックの 30 行のみ。
スライドに出している `src/index.ts` の構造:
- 1 行目: `useServerFn` のエクスポート (React 固有の "実コード")
- 2 行目: `start-client-core` を `export *` で丸ごと再エクスポート
- 4 行目以降 (スライドには省略): 2026-05-21 (commit ce61fa2) に追加された explicit re-export。
  - 理由は Vite SSR cold-start cycle (vitejs/vite#22491) の回避 — `export *` は遅延 bind なので循環参照中に名前が一瞬未定義になる事故が起きるため、public API を明示的に "shadow" して link time に予約する
  - これは Start 側のロジック追加ではなく Vite ランタイムへの防衛策

メッセージ：「`@tanstack/react-start` は start-client-core の React 用 adapter にすぎない」は今も真。
- React 固有のロジックは useServerFn の 30 行に閉じ込められている
- public API (createServerFn 等) は framework 非依存な core 側にある
- だから solid-start も同じ構造で薄く成立する (= framework adapter pattern)
- 質問が来たら: 「2 日前まではこれ literal に 2 行でした」と笑い話に
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
2026-04  Start に react-start-rsc を独立 package 化
       ↓ isomorphic-first / RSC は Query で扱う方向
2026-05  Solid Start は v2 beta 並走 / React Start は 1.x
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
    <span class="font-semibold">SPA + SSR + Hydration</span> のメンタルモデルは依然強力 (初回は SSR、以降はクライアントで view 組立)。全部 RSC でやり直すのは過剰 — 型安全性で進化させ必要な時だけサーバーに委譲
  </div>
</div>

</v-clicks>

<!--
歴史的背景：2020 Next.js getServerSideProps → 2023 RSC でデフォルトが Server-first になった。
タイムライン補足：2025-09 Start v1 RC、2026-04 RSC サポートは `@tanstack/react-start-rsc` という独立 package として monorepo に追加 (react-start の workspace deps に既に組込済 = もはや experimental flag の話ではない)。
2026-05 時点: Solid Start は 2.0 beta シリーズが進行中、React Start は 1.168.x、Vue Start も並走。
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

```tsx {all|2-7|8-12|all}{lines:true}
// 3 つから選ぶ — 同じデッキ内で混在 OK
createFileRoute('/dashboard')({
  component: Dashboard,
  ssr: true,                  // フル SSR (デフォルト)
  // ssr: 'data-only',           // データだけ server
  // ssr: false,                 // 完全 CSR
})
createFileRoute('/canvas')({     // ssr: false → loader からブラウザ API
  component: Canvas,
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
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Definition</p>

# <span class="text-4xl font-extrabold tracking-tight">"client-first" の中身</span>

<p class="text-sm text-neutral-600 mt-2">
  SSR を捨てるではない — <span class="font-bold">思想の中心が client にある</span>こと。Next.js App Router (server-first) と <span class="font-bold">5 軸</span>で対比すると見える。
</p>

<div class="grid grid-cols-12 gap-2 mt-5 text-xs">

<div class="col-span-3 text-neutral-500 font-semibold uppercase tracking-wider text-[10px] pl-2 self-end">軸</div>
<div class="col-span-4 text-neutral-700 font-bold pl-2 self-end">Next.js App Router</div>
<div class="col-span-5 text-cyan-600 font-bold pl-2 self-end">TanStack Start</div>

<div class="col-span-3 bg-neutral-100 p-2 rounded font-semibold">Component モデル</div>
<div class="col-span-4 bg-amber-50 p-2 rounded">Server / Client の <span class="font-bold">2 種類</span></div>
<div class="col-span-5 bg-cyan-50 p-2 rounded"><span class="font-bold">1 種類</span> (普通の React、同型)</div>

<div class="col-span-3 bg-neutral-100 p-2 rounded font-semibold">境界マーク</div>
<div class="col-span-4 bg-amber-50 p-2 rounded"><code>'use client'</code> 必須 (コンポ単位)</div>
<div class="col-span-5 bg-cyan-50 p-2 rounded"><code>ssr</code> フラグ (ルート単位)</div>

<div class="col-span-3 bg-neutral-100 p-2 rounded font-semibold">状態の所有</div>
<div class="col-span-4 bg-amber-50 p-2 rounded">サーバー側</div>
<div class="col-span-5 bg-cyan-50 p-2 rounded">client (URL / Query / useState)</div>

<div class="col-span-3 bg-neutral-100 p-2 rounded font-semibold">型の源泉</div>
<div class="col-span-4 bg-amber-50 p-2 rounded">RSC payload (runtime)</div>
<div class="col-span-5 bg-cyan-50 p-2 rounded">route tree (TypeScript 推論)</div>

<div class="col-span-3 bg-neutral-100 p-2 rounded font-semibold">遷移後の挙動</div>
<div class="col-span-4 bg-amber-50 p-2 rounded">毎回サーバー往復 (RSC)</div>
<div class="col-span-5 bg-cyan-50 p-2 rounded">完全 SPA (loader 分だけ JSON)</div>

</div>

<v-click>

<div class="mt-4 p-3 bg-neutral-900 text-white rounded-lg">
  <span class="text-xs font-semibold tracking-widest uppercase opacity-60">つまり</span>
  <div class="text-sm font-medium mt-0.5">SSR は <span class="text-cyan-400">初回 paint の装飾</span>。思想の中心は <span class="text-cyan-400 font-bold">client</span> にある。</div>
  <div class="text-xs opacity-70 mt-1">※「初回」= ブラウザが <span class="font-semibold">page request</span> するタイミング (新規アクセス / Cmd+R / 外部リンク / クローラ)。<code>&lt;Link&gt;</code> 遷移は <code>ssr: true</code> でも<span class="font-semibold">フル SSR されず、loader も client で実行</span>される</div>
</div>

</v-click>

<!--
Section 02 の締め。「client-first って言うけど Start も SSR デフォルトじゃん」という当然の疑問に 1 枚で答える。
5 軸の対比で「client-first ≠ SSR を捨てる」を示す:
1. Component モデル: Next は Server/Client 2 種類、Start は 1 種類 (isomorphic な普通の React)
2. 境界マーク: Next は 'use client' (コンポ単位)、Start は ssr フラグ (ルート単位)
3. 状態の所有: Next はサーバー側、Start は client (URL / Query / useState)
4. 型の源泉: Next は RSC payload (framework runtime)、Start は route tree (純 TypeScript)
5. 遷移後の挙動: Next は毎回サーバー往復 (RSC fetch)、Start は完全 SPA

→ SSR は両者ともやる。違うのは "思想の中心"。Start は client が boss、server は助手。Next は server が boss、client が opt-in。
Section 03 への橋渡し: この設計だからこそ次章で語る "ドーナツホールがない" "Payload リスクがコードで見える" "型が貫通する" が成立する。
質問対応: "結局 SSR してるのに何が違う？" → "コンポーネントモデルが 1 種類で、思想の中心が client にあること" と一言で返せる。
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

```tsx {all|2-4|6-10|12-14|all}{lines:true}
// page.tsx ── Server (UserCard 用 data を ここで 取得)
async function Page() {
  const user = await db.user.current()             // ← 本来 UserCard の関心事
  return <Modal><UserCard user={user} /></Modal>   /* server→client→server */
}
// Modal.tsx ── 'use client' (state)
'use client'
function Modal({ children }) {
  const [open] = useState(false)
  return open && <div>{children}</div>
}
// UserCard.tsx ── 自分で fetch できない
function UserCard({ user }) {                      // ← data は props 経由
  return <div>{user.name}</div>
}
```

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
layout: two-cols-header
---

<p class="text-xs font-semibold tracking-widest uppercase text-amber-600 mb-2">Payload Cost</p>

# <span class="text-4xl font-extrabold tracking-tight">Props は RSC payload に "焼き付く"</span>

::left::

```tsx {all|2-4|6-7|10-12|all}{lines:true}
// ❌ アンチパターン: app/page.tsx ── Server (やってはいけない例)
async function Page() {
  const scores = await db.score.findMany() // 10 万件
  const apiKey = process.env.OPENAI_KEY!   // サーバー秘密

  // ⚠️ scores も apiKey も RSC payload に焼き込まれ HTML に露出
  return <Chart scores={scores} apiKey={apiKey} />
}

// Chart.tsx ── 'use client'
'use client'
function Chart({ scores, apiKey }) { /* … */ }
```

::right::

## <span class="text-xl font-bold">起きること</span>

<v-clicks>

<div class="bg-amber-50 p-4 rounded-lg mt-3">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">サイズの肥大</div>
  <div class="text-sm mt-1">RSC payload が肥大 → 初回 HTML が <span class="font-bold">100KB → 500KB</span>。bundle と違い毎回送信</div>
</div>

<div class="bg-amber-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">秘匿情報の漏洩</div>
  <div class="text-sm mt-1">Server の変数も初回 HTML に <span class="font-bold">文字列として混入</span></div>
</div>

<div class="bg-amber-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">対策が "規律頼み"</div>
  <div class="text-sm mt-1"><code>taintObjectReference</code> =「Client に渡ったらエラー」化 (experimental)</div>
</div>

</v-clicks>

<v-click>

<p class="mt-4 text-sm text-neutral-700 italic">
  → Start は <code class="text-cyan-600 font-mono">createServerFn</code> で<span class="font-bold text-cyan-600">境界がコードに見える</span>。漏れる経路を実装者が把握できる。
</p>

</v-click>

<!--
chot inc. の Zenn 記事 (https://zenn.dev/chot/articles/e0e2203d12bff8) の論点。
Server Component から Client Component に渡した props は、シリアライズされて RSC payload (= 初回 HTML の <script> タグ) に焼き付けられる。

3 つのコスト：
- サイズ肥大: chot の実測で、1 億要素 (10000²) の配列を props で渡すと payload が 100KB → 500KB へ
- 秘匿情報の漏洩: process.env / DB の "サーバー内に閉じておくべき" 値を Client props に載せると HTML に文字列として登場
- 対策が規律頼み: React の `taintObjectReference` / `taintUniqueValue` は experimental。明示利用しないと何も守らない

slide 16 (ドーナツホール = 認知のホール) → このスライド (payload の物理コスト) → 次スライド (Start なら payload を Query で明示扱い) の流れで、
Server-Client 境界の "見えなさ" がもたらす痛みを 2 種類セットで提示する。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Pattern · React Paris 2026</p>

# <span class="text-4xl font-extrabold tracking-tight">RSC も "ただの非同期データ"</span>

<p class="text-sm text-neutral-600 mt-2">サーバーで <code>renderToReadableStream(&lt;JSX/&gt;)</code>、クライアントで <code>useQuery</code> で受けるだけ。<span class="text-xs text-neutral-400">— <code>@tanstack/react-start-rsc</code> として monorepo 組込済</span></p>

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

<v-click>

<div class="mt-3 px-3 py-2 bg-neutral-900 text-white rounded-lg text-xs">
  <span class="opacity-60">公式 docs:</span> "Server Component output is <span class="text-cyan-400">the same as any other data</span> — <span class="text-cyan-400 font-bold">no special 'component cache'</span>."
  <div class="opacity-80 mt-0.5">→ 「Server Component の出力も他データと同じ扱い — <span class="text-cyan-400 font-semibold">専用の "component cache" は要らない</span>」<span class="opacity-60 ml-1">= staleTime / gcTime / invalidate がそのまま効く</span></div>
</div>

</v-click>

<!--
2026-04 BeJS / React Paris で Tanner Linsley が実演した正式 API。
サーバー側で `renderToReadableStream(<JSX/>)` → クライアント側で `useQuery` の queryFn として呼ぶだけ。
ストリームのデシリアライズはフレームワークが吸収するので、コードに "魔法" や暗黙の境界は現れない。

Tanner の哲学："RSC は サーバー状態にすぎない" → だったら TanStack Query で扱えばいい：
- staleTime / gcTime / refetchInterval などの既存メカニズムがそのまま効く
- queryKey 単位の選択的 invalidation
- Next.js のような予測不可能な「コンポーネント専用キャッシュ層」は不要

データを返せば普通の Server Function、JSX を返せば RSC ストリーム。同じ formula で両方扱える点を強調する。
-->

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Server Routes</p>

# <span class="text-4xl font-extrabold tracking-tight">REST API も同じファイルで</span>

<p class="text-sm text-neutral-600 mt-2">Next.js の Route Handlers と同じ立て方。さらに <span class="font-bold">page route と同居</span>できる。</p>

```ts {all|2-7|9-12|11|12|all}{lines:true}
// === Server: routes/posts.$id.ts === (同居)
export const Route = createFileRoute('/posts/$id')({
  component: PostPage,                              // ← UI 描画
  loader: ({ params }) => db.post.find(params.id),  // ← UI 用 data (型貫通)
  server: { handlers: {
    DELETE: async ({ params }) => { /* … */ },      // ← 外部 REST API
  }},
})
// === Client: PostPage ===
function PostPage() {
  const post = Route.useLoaderData()                                // ✓ Post 型に自動推論
  await fetch(`/posts/${post.id}`, { method: 'DELETE' })            // ⚠️ URL/戻り値は型なし
}
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

# <span class="text-4xl font-extrabold tracking-tight">ルート定義 → 呼び出し側まで <span class="text-cyan-600">型が貫通</span></span>

```ts {all|2-5|8-12|all}{lines:true}
// ルート定義 — 型のソース・オブ・トゥルース
export const Route = createFileRoute('/posts/$postId')({
  loader: ({ params }) => fetchPost(params.postId),
  validateSearch: z.object({ tab: z.enum(['comments', 'likes']) }),
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

<div class="mt-3 px-3 py-1.5 bg-neutral-900 text-white rounded-lg text-xs">
  <span class="opacity-60">公式 docs:</span> "In the age of <span class="text-cyan-400">AI-assisted development</span>, end-to-end type safety should be <span class="text-cyan-400 font-bold">non-negotiable</span>."
  <div class="opacity-80 mt-0.5">→ 「<span class="text-cyan-400">AI 支援開発の時代</span>、end-to-end な型安全性は <span class="text-cyan-400 font-semibold">妥協できない要件</span>」</div>
</div>

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

<p class="text-sm text-neutral-600 mt-1">Zod スキーマ + middleware で、URL を <span class="font-bold">型安全 / 検証付き / 共有可能</span>な状態管理にする。<span class="text-xs text-neutral-400">※ <code>zod-adapter</code> / <code>valibot-adapter</code> / <code>arktype-adapter</code> が monorepo に first-class で並ぶ</span></p>

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
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">More</p>

# <span class="text-4xl font-extrabold tracking-tight">他にもある "Start ならでは"</span>

<p class="text-sm text-neutral-600 mt-2">Next.js / Remix と差がつく細部の API たち。</p>

<div class="grid grid-cols-3 gap-3 mt-4">

<div class="bg-cyan-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-1">Platform</div>
  <div class="text-base font-bold text-neutral-900">Nitro 経由でどこでも</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug"><code>@tanstack/nitro-v2-vite-plugin</code> 経由で <span class="font-semibold">Nuxt と同じ Nitro</span>。Node / Bun / Deno / Workers / Lambda にそのまま乗る。<span class="font-semibold">Vercel 結合なし</span>。</div>
</div>

<div class="bg-emerald-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 mb-1">Composite Components</div>
  <div class="text-base font-bold text-neutral-900">サーバーは枠だけ、中身は client</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug">2026-04 BeJS で披露された IoC パターン。Server が枠組み + データ、client が slot に注入。<span class="font-semibold"><code>'use client'</code> 不要</span>に。</div>
</div>

<div class="bg-amber-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600 mb-1">Speed</div>
  <div class="text-base font-bold text-neutral-900">Intent Preloading</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug"><code>defaultPreload: 'intent'</code> でホバー時に次ルートの loader を先回り。デフォで体感が速い。</div>
</div>

<div class="bg-purple-50 p-3 rounded-lg transition-all duration-200 hover:scale-[1.02]">
  <div class="text-xs font-semibold tracking-widest uppercase text-purple-600 mb-1">"真" の Middleware</div>
  <div class="text-base font-bold text-neutral-900">Next が <code>proxy</code> に改名した中身がこれ</div>
  <div class="text-xs text-neutral-600 mt-1 leading-snug">route と同 runtime で動き DB / npm 自由、型付き context 伝播 + <code>throw redirect()</code>。Next 旧 middleware は Edge 限定 reverse proxy。</div>
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
- Composite Components: 2026-04 BeJS Tanner 披露の IoC パターン。Server が枠組み + データ、client が slot に注入 → `'use client'` 不要。React の Render Props を Network Boundary でやる発想
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
    <li class="flex gap-2"><span class="font-bold">·</span> LLM チャット / AI streaming UI</li>
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
左 (Start が刺さる)：ダッシュボード / 管理画面 / SaaS 認証後 / 社内ツール / 既存 SPA を壊さず移行したい / Type-safe routing 最優先 / LLM チャット (async function* で型付きストリーミング、API キーは server function に隔離)。
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

