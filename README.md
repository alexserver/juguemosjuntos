# Juguemos Juntos - Monorepo

This is a monorepo containing two independent Astro applications for the Juguemos Juntos lotería game website.

## Project Structure

```text
/
├── apps/
│   ├── site-es/        # Spanish version (juguemosjuntos.mx)
│   │   ├── src/
│   │   │   ├── pages/
│   │   │   ├── components/
│   │   │   ├── layouts/
│   │   │   ├── data/
│   │   │   └── assets/
│   │   ├── public/
│   │   ├── package.json
│   │   ├── astro.config.mjs
│   │   └── .env
│   └── site-en/        # English version (juguemosjuntos.com)
│       ├── src/
│       │   ├── pages/
│       │   ├── components/
│       │   ├── layouts/
│       │   ├── data/
│       │   └── assets/
│       ├── public/
│       ├── package.json
│       ├── astro.config.mjs
│       └── .env
├── specs/              # Project specifications
└── package.json        # Root workspace configuration
```

## Why a Monorepo?

This project uses a simple monorepo structure with two completely independent apps:
- **Spanish (site-es)**: Physical products sold in Mexico (120 MXN + shipping)
- **English (site-en)**: Digital PDF downloads for international customers (15 USD)

The monorepo allows:
- ✅ Complete isolation between markets (prevents piracy concerns)
- ✅ Single development context (one VSCode window, one Claude session)
- ✅ Easy maintenance for frequent content updates
- ✅ Flexibility to diverge features independently

## Commands

All commands are run from the project root:

### Development
| Command                | Action                                           |
| :--------------------- | :----------------------------------------------- |
| `npm install`          | Install dependencies for all apps                |
| `npm run dev:es`       | Start Spanish dev server at `localhost:4321`     |
| `npm run dev:en`       | Start English dev server at `localhost:4321`     |
| `npm run dev:all`      | Start both dev servers (requires terminal split) |

### Building
| Command                | Action                                           |
| :--------------------- | :----------------------------------------------- |
| `npm run build:es`     | Build Spanish app to `apps/site-es/dist/`        |
| `npm run build:en`     | Build English app to `apps/site-en/dist/`        |
| `npm run build:all`    | Build both apps                                  |

### Preview
| Command                | Action                                           |
| :--------------------- | :----------------------------------------------- |
| `npm run preview:es`   | Preview Spanish production build                 |
| `npm run preview:en`   | Preview English production build                 |

## Deployment

Each app is deployed to a separate domain:
- **Spanish**: Deploy `apps/site-es` to `juguemosjuntos.mx` (or `.com.mx`)
- **English**: Deploy `apps/site-en` to `juguemosjuntos.com` (or `.us`)

Configure your hosting provider (Netlify, Vercel, etc.) to:
1. Create two separate sites/projects
2. Point each to the same repository
3. Set different base directories (`apps/site-es` vs `apps/site-en`)
4. Configure environment variables per site
5. Set up custom domains

## Making Changes

### Updating Both Apps
When making changes that apply to both apps (e.g., UI fixes, component updates):

1. Make the change in `apps/site-es`
2. Test it works
3. Copy the change to `apps/site-en`
4. Test the English version

Or ask Claude Code to update both apps in one command:
```
"Update the CTA button color to green in both apps"
```

### Updating One App
Each app can be updated independently:
- Spanish-specific changes go in `apps/site-es`
- English-specific changes go in `apps/site-en`

## Documentation

- See `CLAUDE.md` for detailed architecture and development guidelines
- See `specs/` directory for project specifications
- See `specs/06-monorepo-i18n.md` for monorepo implementation details

## Learn More

- [Astro Documentation](https://docs.astro.build)
- [Astro Discord](https://astro.build/chat)
- [npm Workspaces](https://docs.npmjs.com/cli/v7/using-npm/workspaces)
