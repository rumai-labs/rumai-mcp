# Review notes: annotation hints

For the OpenAI plugin review (IS-HL.10 row 9c, section 21 item 5). The plugin manifest has no field for these reasons, so the owner pastes them in the portal if it asks.

Every tool of the MCP server at `https://api.rumai.farm/mcp` must declare the three hints as explicit booleans. Each row gives the value the tool must declare and the reason found in the code of `services/interactions/src/delivery/`.

**Current state, checked 2026-11-18:** each of the nine tools declares only `readOnlyHint: true`. `destructiveHint: false` and `openWorldHint: false` are not yet declared. Row 9a adds them. The reasons below hold for the code as it is; they are the reasons to write into the portal once row 9a lands.

## Common reasons (all nine tools)

- **`readOnlyHint: true`.** The tool declares `annotations: { readOnlyHint: true }`. Its handler calls only a query port (`*Query`, `FarmCallerContextPort`) and returns the result. Checked: no `insert`, `update` or `delete` call (Kysely `insertInto`, `updateTable`, `deleteFrom`, or SQL `INSERT`/`UPDATE`/`DELETE`) in `services/interactions/src/application` or `services/interactions/src/delivery`, outside tests.
- **`destructiveHint: false`.** The tool has no write path, so it removes or changes no data. Same check as above.
- **`openWorldHint: false`.** The tool calls no service outside RUMAI. Checked: no `fetch(`, `axios` or `HttpService` in the `*.tool.ts` files. Its data comes from RUMAI's own database through the query ports, for farms the person belongs to.

## Per tool

| Tool | Declared in code | `readOnlyHint` | `destructiveHint` | `openWorldHint` | Reason from the code |
|---|---|---|---|---|---|
| `who_am_i` | `who-am-i.tool.ts:25` | true | false | false | Reads the person's name, email and farms from the verified token and the farm membership. Writes nothing. Calls no external service. |
| `farm_summary` | `farm-summary.tool.ts:35` | true | false | false | Counts a farm's active animals and open alerts through its query port, after the membership check. Writes nothing. |
| `herd_list` | `herd-list.tool.ts:47` | true | false | false | Pages a farm's animals with filters (state, sex, category, tags, name). Uses the membership check and the query port. Writes nothing. |
| `animal_detail` | `animal-detail.tool.ts:38` | true | false | false | Reads the card of up to 50 animals by id from the farm, after the membership check. Its photo is an address already stored, not fetched by the tool. Writes nothing. |
| `farm_query` | `farm-query.tool.ts:88` | true | false | false | Reads one set (animals, alerts or records) of a farm, filtered or counted. Writes nothing. |
| `open_alerts` | `open-alerts.tool.ts:45` | true | false | false | Lists a farm's open alerts, newest first, a page at a time. Writes nothing. |
| `record_history` | `record-history.tool.ts:48` | true | false | false | Lists one animal's effective records, newest first, a page at a time. Writes nothing. |
| `reproductive_state` | `reproductive-state.tool.ts:46` | true | false | false | Reads the state and dates the rules derive for up to 50 animals, from the stored records. Writes nothing. |
| `reproductive_summary` | `reproductive-summary.tool.ts:42` | true | false | false | Counts a farm's services, pregnancy checks and calvings over a period, by method, result and sire. Writes nothing. |

## Notes for the reviewer

- The tools that change data (none in this server) would set `readOnlyHint: false`. The server has none. The consent text says the same («Esta aplicación podrá leer tu nombre, tu correo y los datos de las fincas a las que perteneces»).
- Not checked here: the query ports' own adapters in `services/herd`, `services/alerts` and `services/reproduction`. The check above covers the interactions application and delivery code the tools call directly. Row 9c's reviewer should confirm each adapter is read-only before submission.
- Test cases run on the reviewer account, against Granja RUMAI Demo, are not verified here (owner step, spec section 21 item 7).
