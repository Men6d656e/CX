# Apps

One git submodule per application. Code lives only here, never in the engine repo.

| App | Remote | Purpose | How to run |
|-----|--------|---------|------------|
| vite_express_web | https://github.com/Men6d656e/vite_express_web.git | Single login + posts CRUD demo (Express backend + Vite/React/Tailwind frontend) | `cd vite_express_web/backend && npm install && npm run dev` (port 4000), then `cd vite_express_web/frontend && npm install && npm run dev` (port 5173). See the app's own README. |
| vite_express_web_mongodb | https://github.com/Men6d656e/vite_express_web_mongodb.git | v1 replica with MongoDB: register/login (JWT cookie + bearer), posts CRUD, like/dislike, react-router frontend | `cd vite_express_web_mongodb/backend && npm install`, set `MONGODB_URI` + `JWT_SECRET` in `.env`, then `npm start` (prod) or `npm run dev`. See the app's own README. |
