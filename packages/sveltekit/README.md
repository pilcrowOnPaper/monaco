# `@monaco-auth/sveltekit`

SvelteKit integration for Monaco. For a full list of OAuth providers, see [`@monaco-auth/core`](https://github.com/pilcrowOnPaper/monaco/tree/main/packages/core).

See the [SvelteKit example](https://github.com/pilcrowOnPaper/monaco/tree/main/examples/sveltekit) for details.

## Installation

```
npm install @monaco-auth/sveltekit
```

## Usage

### `.env`

Define the app url. If you define a path, all auth routes will be defined within it (e.g. `/auth/login/github`).

```bash
MONACO_BASE_URL="http://localhost:5173"
```

### Hooks

```ts
// src/hooks.server.ts
import { PrismaAdapter } from "@monaco-auth/adapter-prisma";
import { monacoSvelteKit } from "@monaco-auth/sveltekit";
import { GitHubProvider } from "@monaco-auth/core/providers";
import { PrismaClient } from "@prisma/client";
import { dev } from "$app/environment";
import { GITHUB_CLIENT_ID, GITHUB_CLIENT_SECRET } from "$env/static/private";

import type { Handle } from "@sveltejs/kit";

const client = new PrismaClient();

export const handle: Handle = monacoSvelteKit(new PrismaAdapter(client), {
	dev,
	providers: [new GitHubProvider(GITHUB_CLIENT_ID, GITHUB_CLIENT_SECRET)]
});
```

### Login page

Create a login page in `src/routes/login/+page.svelte`.

See [`@monaco-auth/core` providers section](https://github.com/pilcrowOnPaper/monaco/tree/main/packages/core#providers) for each provider's login URL.

```svelte
<h1>Sign in</h1>
<a href="/login/github">Sign in with Github</a>
```

### Logout page

A logout endpoint is automatically created.

```svelte
<form method="post" action="/logout"  >
	<button>Sign out</button>
</form>
```