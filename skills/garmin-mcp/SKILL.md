---
name: garmin-mcp
description: Skill da REST API do Garmin Connect (treinos) na MCP.AI: 10 endpoints em /api/garmin. Listar, obter, criar, editar (substituir), apagar e agendar treinos estruturados no Garmin Connect (corrida, ciclismo, natação, multi-sport / triatlo). Usa a API não oficial via sessão Garmin (email + senha); tokens OAuth são guardados encriptados para reduzir logins. Autentique com workspace API key (sk_live) gerada em app.mcp.ai/settings/api-keys. Use quando o usuário pedir algo coberto pelos endpoints.
---

# Garmin Connect (treinos) — REST API skill

Você tem acesso à **Garmin Connect (treinos)** REST API na MCP.AI.

> Listar, obter, criar, editar (substituir), apagar e agendar treinos estruturados no Garmin Connect (corrida, ciclismo, natação, multi-sport / triatlo). Usa a API não oficial via sessão Garmin (email + senha); tokens OAuth são guardados encriptados para reduzir logins.

## Base URL

```
https://api.mcp.ai/api/garmin
```

Todo endpoint é um **POST** na Base URL + o path abaixo. Os parâmetros vão no corpo JSON.

## Autenticação

Inclua em toda request:

```
Authorization: Bearer sk_live_...
Content-Type: application/json
```

> Gere sua chave em **https://app.mcp.ai/settings/api-keys** (workspace API key `sk_live_…`, não expira, revogável). Uma única chave serve pra todos os seus MCPs.

## Formato de resposta

```json
{ "ok": true, "tool": "<tool_id>", "result": <payload> }
```

## Exemplo cURL

```bash
curl -X POST https://api.mcp.ai/api/garmin/delete/workout \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -d '{"workout_id":"..."}'
```

## Reportar problemas

Se um endpoint retornar erro, vazio ou dado inesperado, reporte (não desista calado): **POST /api/garmin/report** com `{ "message": "...", "context"?: "...", "conversation"?: [...] }`. Isso notifica o time da MCP.AI.

## Endpoints (10)

#### `garmin_delete_workout`

Delete a workout template from Garmin Connect by workout_id. _(POST /api/garmin/delete/workout)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `workout_id` | string | Sim |  |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |
| `workout_ids` | string[] | Não | Bulk mode: multiple values for workout_id |

#### `garmin_edit_workout`

Replace a workout: uploads a new workout from payload, then deletes workout_id (no native PUT). Returns new workout_id and previous_workout_id. _(POST /api/garmin/edit/workout)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `workout_id` | string | Sim | Existing Garmin workout to replace |
| `workout` | object | Sim | Full Garmin workout JSON for POST /workout-service/workout. Required top-level: workoutName, sportType {sportTypeId, sportTypeKey}, workoutSegments[{segmentOrder, sportType, workoutSteps}]. Optional: description, estimatedDurationInSecs, author. Step kinds: ExecutableStepDTO { type, stepOrder, stepType{stepTypeId,stepTypeKey}, endCondition{conditionTypeId,conditionTypeKey}, endConditionValue?, targetType?{workoutTargetTypeId,workoutTargetTypeKey}, targetValueOne?, targetValueTwo? } or RepeatGroupDTO { type, stepOrder, stepType{stepTypeId,stepTypeKey}, numberOfIterations, workoutSteps[] }. `displayOrder` (sportType/stepType/endCondition/targetType) and `displayable` (endCondition) are auto-filled server-side when omitted, as is RepeatGroupDTO.endCondition (defaults to conditionTypeKey=iterations) and RepeatGroupDTO.endConditionValue (mirrors numberOfIterations) — do NOT pass them unless you want a non-default value. Units: pace targets in m/s (e.g. 5:00 min/km ≈ 3.3333); time endConditionValue in seconds; distance in meters. |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |
| `workout_ids` | string[] | Não | Bulk mode: multiple values for workout_id |

#### `garmin_get_scheduled_workout`

Get one scheduled workout entry by scheduled_workout_id. _(POST /api/garmin/get/scheduled/workout)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `scheduled_workout_id` | string | Sim |  |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |
| `scheduled_workout_ids` | string[] | Não | Bulk mode: multiple values for scheduled_workout_id |

#### `garmin_get_workout`

Get one workout template by workout_id (full structure: segments, steps, targets). Includes raw_data JSON string. _(POST /api/garmin/get/workout)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `workout_id` | string | Sim | Garmin workoutId from list or submit response |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |
| `workout_ids` | string[] | Não | Bulk mode: multiple values for workout_id |

#### `garmin_list_accounts`

List all Garmin logins linked to this install (id, email, label, created_at). _(POST /api/garmin/list/accounts)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |

#### `garmin_list_scheduled_workouts`

List calendar scheduled workouts for a given month/year from Garmin calendar-service. _(POST /api/garmin/list/scheduled/workouts)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `year` | integer | Sim | Calendar year (e.g. 2026) |
| `month` | integer | Sim | Calendar month 1-12 |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |

#### `garmin_list_workouts`

List saved workouts from Garmin Connect (workout-service). Returns summaries plus raw_data (full JSON string from Garmin). _(POST /api/garmin/list/workouts)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `start` | integer | Não | Pagination start index |
| `limit` | integer | Não | Max workouts to return (1–100) |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |

#### `garmin_schedule_workout`

Schedule an existing workout onto the calendar. date must be YYYY-MM-DD (or ISO-8601 date prefix). Returns Garmin schedule response as raw_data. _(POST /api/garmin/schedule/workout)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `workout_id` | string | Sim |  |
| `date` | string | Sim | Calendar date e.g. "2026-04-20" or full ISO starting with YYYY-MM-DD |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |
| `workout_ids` | string[] | Não | Bulk mode: multiple values for workout_id |

#### `garmin_submit_workout`

Create a new structured workout on Garmin Connect. Body must match Garmin API (workoutName, sportType, workoutSegments with ExecutableStepDTO / RepeatGroupDTO). You may omit displayOrder/displayable e _(POST /api/garmin/submit/workout)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `workout` | object | Sim | Full Garmin workout JSON for POST /workout-service/workout. Required top-level: workoutName, sportType {sportTypeId, sportTypeKey}, workoutSegments[{segmentOrder, sportType, workoutSteps}]. Optional: description, estimatedDurationInSecs, author. Step kinds: ExecutableStepDTO { type, stepOrder, stepType{stepTypeId,stepTypeKey}, endCondition{conditionTypeId,conditionTypeKey}, endConditionValue?, targetType?{workoutTargetTypeId,workoutTargetTypeKey}, targetValueOne?, targetValueTwo? } or RepeatGroupDTO { type, stepOrder, stepType{stepTypeId,stepTypeKey}, numberOfIterations, workoutSteps[] }. `displayOrder` (sportType/stepType/endCondition/targetType) and `displayable` (endCondition) are auto-filled server-side when omitted, as is RepeatGroupDTO.endCondition (defaults to conditionTypeKey=iterations) and RepeatGroupDTO.endConditionValue (mirrors numberOfIterations) — do NOT pass them unless you want a non-default value. Units: pace targets in m/s (e.g. 5:00 min/km ≈ 3.3333); time endConditionValue in seconds; distance in meters. |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |

#### `garmin_unschedule_workout`

Remove workout from calendar by scheduled_workout_id (does not delete the workout template). _(POST /api/garmin/unschedule/workout)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `scheduled_workout_id` | string | Sim |  |
| `account` | string | Não | Optional account when multiple Garmin connections are linked. Pass id, email, or label. Omit if one. Use garmin_list_accounts. |
| `scheduled_workout_ids` | string[] | Não | Bulk mode: multiple values for scheduled_workout_id |

---

Este MCP também funciona via **conexão MCP** (Claude / Cursor) em `https://api.mcp.ai/p_garmin` — veja o [README](../../README.md). A skill acima é pra consumir a **REST API** direto (agente próprio / código).
