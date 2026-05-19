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

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Fact 01</p>

# <span class="text-4xl font-extrabold tracking-tight"><code class="text-cyan-600 font-mono">TanStack/start</code> は存在しない</span>

<p class="text-neutral-600 mt-3">独立リポジトリを叩くと <code>404 Not Found</code> が返る。Start の開発は <code>TanStack/router</code> 内で進められている。</p>

<div class="grid grid-cols-2 gap-6 mt-8">

<div class="bg-neutral-900 text-white p-6 rounded-lg font-mono text-sm">
<div class="text-xs font-semibold tracking-widest uppercase text-red-400 mb-2">github.com/TanStack/start</div>

```text
404
Page not found
```

<div class="text-xs text-neutral-400 font-sans mt-3">→ Start という独立リポジトリは存在しない</div>

</div>

<div class="bg-cyan-50 p-6 rounded-lg font-mono text-sm">
<div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">github.com/TanStack/router</div>

```text
description: "🤖 A client-first,
  server-capable, fully type-safe
  router and full-stack framework
  for the web (React and more)."

stars: 14.5k+  forks: 1.7k+
```

<div class="text-xs text-neutral-500 font-sans mt-3">→ 自己紹介に "client-first" / Start は <span class="font-semibold">React と Solid を第一級サポート</span></div>

</div>

</div>

<v-click>

<p class="mt-6 text-lg font-bold text-neutral-900">
  公式の自己定義からして <span class="text-cyan-600">"client-first"</span> が含まれている。
</p>

</v-click>

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Fact 02</p>

# <span class="text-4xl font-extrabold tracking-tight">README が明言している</span>

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

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Fact 03</p>

# <span class="text-4xl font-extrabold tracking-tight">packages/ の中で同居している</span>

<p class="text-neutral-600 mt-3"><code>TanStack/router/packages/</code> には Router と Start が並んで存在する（40+ packages）。</p>

<div class="grid grid-cols-2 gap-4 mt-6 text-sm font-mono">

<div class="bg-emerald-50 p-5 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 mb-3">Router 系</div>
  <div class="space-y-1 text-neutral-700">
    <div>router-core</div>
    <div>react-router</div>
    <div>solid-router</div>
    <div>vue-router</div>
    <div>router-cli / router-plugin</div>
    <div>router-generator</div>
    <div>router-vite-plugin</div>
    <div>router-devtools-core / -devtools</div>
    <div>router-utils</div>
    <div class="text-neutral-400 mt-2 text-xs">… SSR/Query adapters も同居</div>
  </div>
</div>

<div class="bg-cyan-50 p-5 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Start 系</div>
  <div class="space-y-1 text-neutral-700">
    <div>start-client-core</div>
    <div>start-server-core</div>
    <div>start-plugin-core</div>
    <div>start-fn-stubs</div>
    <div>start-storage-context</div>
    <div>start-static-server-functions</div>
    <div>react-start / -client / -server / -rsc</div>
    <div>solid-start / -client / -server</div>
    <div class="text-neutral-400 mt-2 text-xs">… vue-start も同居 (実験的)</div>
  </div>
  <div class="text-xs text-neutral-500 font-sans mt-3">第一級は <span class="font-semibold text-cyan-600">React / Solid</span> の 2 系統</div>
</div>

</div>

<v-click>

<p class="mt-5 text-sm text-neutral-600">
  Start は <span class="font-bold text-cyan-600">"Router のサブプロジェクト"</span> として、同じ monorepo で開発・リリースされている。
</p>

</v-click>

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Fact 04</p>

# <span class="text-4xl font-extrabold tracking-tight">レイヤー構造</span>

<p class="text-neutral-600 mt-3"><code>@tanstack/react-start</code> の依存はすべて workspace 内パッケージ。外部依存は <code>pathe</code> 1つだけ。</p>

<div class="bg-gray-100 p-6 rounded-lg font-mono text-sm leading-relaxed mt-6">

```text
                  react-start  (薄い framework エントリ)
                       ↓
                start-client-core  (server function 機構)
                       ↓
                  router-core  (framework-agnostic)
                       ↑
                  react-router  (React bindings)
```

</div>

<div class="grid grid-cols-3 gap-4 mt-6">

<v-click>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">react-start</div>
  <div class="text-sm mt-1 text-neutral-700">SSR / RSC / プラグイン入り口</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">start-client-core</div>
  <div class="text-sm mt-1 text-neutral-700">server function スタブ / stream</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">router-core</div>
  <div class="text-sm mt-1 text-neutral-700">本体 — ルートマッチ / ロード</div>
</div>

</v-click>

</div>

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Fact 05 · Routing</p>

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

---
layout: default
class: 'bg-gray-100'
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Numbers</p>

# <span class="text-4xl font-extrabold tracking-tight">"90% は Router" を実測する</span>

<p class="text-lg text-neutral-600 mt-3 max-w-3xl">
  リポジトリ内の <code>react-router/src</code> と <code>react-start/src</code> を実測比較。
</p>

<div class="grid grid-cols-4 gap-8 mt-12 border-t-2 border-neutral-300 pt-8">
  <div>
    <div class="text-5xl font-black tracking-tight text-emerald-500">45</div>
    <div class="text-xs font-semibold tracking-widest uppercase text-neutral-500 mt-2">react-router files</div>
  </div>
  <div>
    <div class="text-5xl font-black tracking-tight text-emerald-500">187<span class="text-2xl">KB</span></div>
    <div class="text-xs font-semibold tracking-widest uppercase text-neutral-500 mt-2">router src bytes</div>
  </div>
  <div>
    <div class="text-5xl font-black tracking-tight text-cyan-500">12</div>
    <div class="text-xs font-semibold tracking-widest uppercase text-neutral-500 mt-2">react-start files</div>
  </div>
  <div>
    <div class="text-5xl font-black tracking-tight text-cyan-500">2.5<span class="text-2xl">KB</span></div>
    <div class="text-xs font-semibold tracking-widest uppercase text-neutral-500 mt-2">start src bytes</div>
  </div>
</div>

<v-click>

<div class="mt-8 p-5 bg-neutral-900 text-white rounded-lg flex items-center justify-between">
  <div>
    <div class="text-xs font-semibold tracking-widest uppercase opacity-60">react-start src ÷ react-router src</div>
    <div class="text-3xl font-black tracking-tight mt-1">≒ <span class="text-cyan-400">1.4%</span></div>
  </div>
  <div class="text-right">
    <div class="text-xs font-semibold tracking-widest uppercase opacity-60">逆算</div>
    <div class="text-3xl font-black tracking-tight mt-1">Router は Start の <span class="text-amber-400">約 73 倍</span></div>
  </div>
</div>

</v-click>

<v-click>

<p class="mt-4 text-sm text-neutral-600">
  ※ src ディレクトリの比較。<code>start-client-core</code> 等を含めても Router が圧倒的に重い。
</p>

</v-click>

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Code · The Smoking Gun</p>

# <span class="text-4xl font-extrabold tracking-tight"><code class="font-mono text-cyan-600">react-start/src/index.ts</code></span>

<p class="text-neutral-600 mt-3"><code>@tanstack/react-start</code> のメインエントリは、これだけ。</p>

```ts {all|1|2|all}{lines:true}
export { useServerFn } from './useServerFn'
export * from '@tanstack/start-client-core'
```

<div class="grid grid-cols-2 gap-4 mt-6">

<v-click>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">L1</div>
  <div class="text-sm mt-1 text-neutral-700">クライアント側で server function を呼ぶラッパー (35 行)</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">L2</div>
  <div class="text-sm mt-1 text-neutral-700"><code>start-client-core</code> を丸ごと再エクスポート</div>
</div>

</v-click>

</div>

<v-click>

<div class="mt-6 p-5 bg-neutral-900 text-white rounded-lg">
  <span class="text-xs font-semibold tracking-widest uppercase opacity-60">結論</span>
  <div class="text-lg font-bold mt-1">Start は "Router + server function 機構" を<span class="text-cyan-400"> Vite プラグインで束ねただけ</span></div>
</div>

</v-click>

---
layout: section
class: 'bg-neutral-900 text-white'
---

<div class="absolute top-12 right-16 w-56 h-56 rounded-full bg-cyan-500/10"></div>
<div class="absolute bottom-16 left-20 w-40 h-40 rounded-lg bg-amber-500/10 rotate-12"></div>

<p class="text-sm font-semibold tracking-widest uppercase opacity-60 mb-4">Section 02 · Philosophy</p>

# <span class="text-7xl font-black tracking-tighter">Client-first</span>

<p class="text-xl font-medium mt-6 opacity-80 max-w-3xl">
  公式が <code class="bg-white/10 px-2 py-0.5 rounded">CRITICAL</code> と書いた設計思想
</p>

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

---
layout: two-cols-header
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">History</p>

# <span class="text-4xl font-extrabold tracking-tight">Server-first 全盛 → Client-first 回帰</span>

::left::

<div class="bg-gray-100 p-5 rounded-lg font-mono text-sm leading-relaxed mt-4">

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
2026-05  まだ RC (1.0 日付は未公表)
```

</div>

<p class="text-xs text-neutral-500 mt-2 leading-snug">
  ※ <code>@tanstack/react-start</code> の <span class="font-semibold">package version は 1.x</span> だが、公式サイト上の <span class="font-semibold">product status はまだ RC</span>。混同に注意。
</p>

::right::

## <span class="text-xl font-bold">Tanner Linsley の主張</span>

<v-clicks>

<div class="bg-cyan-50 p-4 rounded-lg mt-3">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">命名批判</div>
  <div class="text-sm mt-1">RSC は本当は "Pre-render Components"</div>
</div>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">本質</div>
  <div class="text-sm mt-1">RSC = <code>AsyncIterable&lt;string&gt;</code> という単純な primitive</div>
</div>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">姿勢</div>
  <div class="text-sm mt-1">"SPA の歴史を捨てるのではなく磨く"</div>
</div>

</v-clicks>

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Code · ssr: false</p>

# <span class="text-4xl font-extrabold tracking-tight">クライアントに寄せる opt-out</span>

<p class="text-neutral-600 mt-3">Start は初期マッチを SSR するのがデフォルト。<code>ssr: false</code> を書くと、loader もコンポーネントも完全にクライアント実行に opt-out できる。</p>

```tsx {all|2|3-8|9|all}{lines:true}
export const Route = createFileRoute('/canvas')({
  ssr: false,                              // ← クライアント専用
  loader: async () => {
    const saved = localStorage.getItem('canvas-state')
    return {
      Tools: await getDrawingTools({ data: { savedState: saved } }),
    }
  },
  component: CanvasPage,
})
```

<div class="grid grid-cols-3 gap-4 mt-6">

<v-click>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">境界</div>
  <div class="text-sm mt-1"><span class="font-mono text-xs">'use client'</span> より<br/>ルート単位で明示</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">Browser API</div>
  <div class="text-sm mt-1"><code>localStorage</code> / <code>window</code> を<br/>loader から直接</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">SSR との混在</div>
  <div class="text-sm mt-1">同じデッキ内に SSR ルートと<br/>共存できる</div>
</div>

</v-click>

</div>

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

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Selective SSR</p>

# <span class="text-4xl font-extrabold tracking-tight">SSR は "選ぶ" もの</span>

<p class="text-neutral-600 mt-3">ルートごとに <code>ssr</code> オプションで実行場所を制御できる。</p>

```tsx {all|2|3|4|all}{lines:true}
export const Route = createFileRoute('/dashboard')({
  ssr: true,                          // フル SSR
  // ssr: false,                      // 完全 CSR
  // ssr: 'data-only',                // データのみ server、UI は client
  loader: () => fetchDashboard(),
  component: Dashboard,
})
```

<div class="grid grid-cols-3 gap-4 mt-6">

<v-click>

<div class="bg-cyan-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">true</div>
  <div class="text-sm mt-1">loader + render <br/>共に server</div>
</div>

</v-click>

<v-click>

<div class="bg-amber-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-amber-600">'data-only'</div>
  <div class="text-sm mt-1">loader は server、<br/>UI は client</div>
</div>

</v-click>

<v-click>

<div class="bg-emerald-50 p-4 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600">false</div>
  <div class="text-sm mt-1">どちらも client</div>
</div>

</v-click>

</div>

<v-click>

<p class="mt-6 text-base font-medium text-neutral-700">
  Router 単体は client-first。Start は SSR がデフォルトだが、<code>defaultSsr: false</code> / <code>ssr: false</code> / SPA mode で<span class="font-bold text-cyan-600">必要なだけ client-first に寄せられる</span>。
</p>

</v-click>

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

---
layout: two-cols-header
---

<p class="text-xs font-semibold tracking-widest uppercase text-amber-600 mb-2">Donut Hole</p>

# <span class="text-4xl font-extrabold tracking-tight">RSC が生む "認知のホール"</span>

::left::

<div class="bg-gray-100 p-5 rounded-lg font-mono text-sm leading-relaxed mt-4">

```text
Server Component (page.tsx)
  ┌─────────────────────────┐
  │ const data = await db() │ ← server
  │                         │
  │   <ClientChart />       │ ← client (穴)
  │                         │
  │ const more = await ...  │ ← server
  └─────────────────────────┘

  どっちで実行しているのか、
  毎行考えながら書く必要がある
```

</div>

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

---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-2">Pattern</p>

# <span class="text-4xl font-extrabold tracking-tight">createServerFn + useQuery</span>

<p class="text-sm text-neutral-600 mt-2">サーバー処理を <span class="font-bold">普通の関数</span>として書き、クライアントは <code>useQuery</code> で呼ぶ。境界はインポートだけ。</p>

```tsx {all|2-6|9-13|all}{lines:true}
// server: シリアライズされる "ただの async 関数"
const fetchPost = createServerFn({ method: 'GET' })
  .inputValidator((id: string) => id)
  .handler(async ({ data: id }) => {
    return await db.post.findUnique({ where: { id } })
  })
// client: TanStack Query で server 状態として扱う
const { data } = useQuery({
  queryKey: ['post', postId],
  queryFn: () => fetchPost({ data: postId }),
  staleTime: 10 * 60 * 1000,
})
```

<div class="grid grid-cols-3 gap-3 mt-3">

<v-click>

<div class="bg-cyan-50 p-2 rounded-lg">
  <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600">No magic</div>
  <div class="text-xs mt-0.5">RSC のような特別な層なし</div>
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
  <div class="text-xs mt-0.5">Query が一元管理</div>
</div>

</v-click>

</div>

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
