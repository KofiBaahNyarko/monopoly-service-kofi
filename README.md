# CS 262 Monopoly Web Service

This is the data service application for the
[CS 262 sample Monopoly project](https://github.com/calvin-cs262-organization/monopoly-project),
which is deployed here:

- <https://kofi-web-app-aea4aubvcwcaefcj.westus3-01.azurewebsites.net/>

## API Endpoints

### General
- `GET /` &mdash; Returns a hello message (health check)

### Players Endpoints
- `GET /players` &mdash; Returns the full list of players
- `GET /players/:id` &mdash; Returns the single player with the given ID (e.g., `/players/1`)
- `PUT /players/:id` &mdash; Updates a player with the given ID
- `POST /players` &mdash; Creates a new player
- `DELETE /players/:id` &mdash; Deletes a player with the given ID

### Games Endpoints
- `GET /games` &mdash; Returns the full list of games
- `GET /games/:id` &mdash; Returns the name and score for every player who played in the specified game (e.g., `/games/2`)
- `DELETE /games/:id` &mdash; Deletes a game with the given ID (cascades to remove associated PlayerGame records)

## Error Responses

- Invalid endpoints (e.g., `/blob`) &mdash; Return a 404 not-found error
- Invalid IDs (e.g., `/players/-1` or `/games/999`) &mdash; Return a 404 not-found error
- Server errors &mdash; Return a 500 internal server error (details logged server-side only)

It is based on the [standard Azure App Service tutorial for Node.js](https://learn.microsoft.com/en-us/azure/app-service/quickstart-nodejs?tabs=linux&pivots=development-environment-cli).

The database is relational with the schema specified in the `sql/` sub-directory
and is hosted on [Azure PostgreSQL](https://azure.microsoft.com/en-us/products/postgresql/).
The database server, user and password are stored as Azure application settings so that they
aren&rsquo;t exposed in this (public) repo.

## Local Development

To run the service locally:

1. Install dependencies: `npm install`
2. Set up environment variables in `.env` (see `src/monopolyDirect.ts` for the required variables)
3. Source the environment: `source .env`
4. Start the service: `npm start`

The service will run on `http://localhost:3000` (or the port specified in `PORT` environment variable).

## Deployment

This service is configured with GitHub Actions for CI/CD. Pushing to the `main` branch automatically deploys to Azure. See `DEPLOYMENT.md` for detailed deployment instructions.

We implement this sample service as a separate repo to simplify Azure integration;
it&rsquo;s easier to auto-deploy a separate repo to Azure. For your team project&rsquo;s
data service, configure your Azure App Service to auto-deploy from the master/main branch
of your service repo. See the settings for this in the &ldquo;Deployment Center&rdquo;
on your Azure service dashboard.
