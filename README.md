# 📜 Ikigai App Constellation — Technical Document

This document defines the **structure, schema, and relationships** of the Ikigai App Constellation. It is intended as a foundation for development, ensuring all apps (the Six Relics) can share a common data model and ecosystem.

---

## 🌌 Overview

The Ikigai Constellation is composed of six modular apps (Forge, Loom, Ledger, Compass, Chalice, Anvil). Each connects to a shared backend, centered on **Ideas** and **Projects**. Supporting entities (Reflections, Lore, Mechanics, Materials, Quests) extend functionality specific to each app.

---

## 🛠 Platform Strategy

- **Primary Platform:** Mobile-first (Android/iOS).
- **Secondary Platform:** Web/desktop for deep creative work.
- **Optional:** Progressive Web App (offline-capable).

**Frontend:** Flutter or React Native.

**Backend:** Firebase or Supabase (real-time sync + auth).

**Design:** Offline-first, with local storage + cloud sync.

---

## 🗂 Database Schema

### 1. Users
| Field | Type | Notes |
|-------|------|-------|
| user_id | UUID (PK) | Primary key |
| name | String | Display name |
| email | String | Unique |
| password_hash | String | Authentication |
| created_at | Timestamp | |
| settings | JSON | Preferences |

---

### 2. Ideas
| Field | Type | Notes |
|-------|------|-------|
| idea_id | UUID (PK) | Unique idea identifier |
| user_id | UUID (FK) | Belongs to user |
| type | Enum | (story, craft, reflection, game, prompt, misc) |
| content_text | Text | Written content |
| content_media | JSON | File refs (sketches, audio, photos) |
| tags | Array | Classification labels |
| source_app | Enum | (Forge, Loom, Ledger, Compass, Chalice, Anvil) |
| created_at | Timestamp | |
| updated_at | Timestamp | |

---

### 3. Projects
| Field | Type | Notes |
|-------|------|-------|
| project_id | UUID (PK) | |
| user_id | UUID (FK) | |
| name | String | Project name |
| description | Text | |
| type | Enum | (novel, woodworking, game, painting, mixed) |
| status | Enum | (planning, active, paused, complete) |
| created_at | Timestamp | |
| updated_at | Timestamp | |

---

### 4. Project–Idea Links (Join Table)
| Field | Type | Notes |
|-------|------|-------|
| project_id | UUID (FK) | Links to project |
| idea_id | UUID (FK) | Links to idea |
| role | Enum | (seed, note, asset, reflection) |

---

### 5. Reflections (Ikigai Compass)
| Field | Type | Notes |
|-------|------|-------|
| reflection_id | UUID (PK) | |
| user_id | UUID (FK) | |
| idea_id | UUID (FK) | Optional link to captured idea |
| date | Date | |
| prompt | Text | Daily guiding question |
| response | Text | User’s entry |
| alignment_score | Int (1–10) | Optional rating |

---

### 6. Lore (Story Loom + Game Anvil)
| Field | Type | Notes |
|-------|------|-------|
| lore_id | UUID (PK) | |
| project_id | UUID (FK) | |
| type | Enum | (character, setting, item, theme) |
| name | String | |
| description | Text | |
| linked_ideas | Array | References to idea_ids |

---

### 7. Mechanics (Gamecrafter’s Anvil)
| Field | Type | Notes |
|-------|------|-------|
| mechanic_id | UUID (PK) | |
| project_id | UUID (FK) | |
| name | String | |
| description | Text | Rule or mechanic description |
| probability_data | JSON | Dice/card simulation parameters |

---

### 8. Materials (Maker’s Ledger)
| Field | Type | Notes |
|-------|------|-------|
| material_id | UUID (PK) | |
| user_id | UUID (FK) | |
| name | String | Material name |
| quantity | Int | |
| unit | String | (board feet, liters, etc.) |
| notes | Text | Source, cost, supplier |
| last_updated | Timestamp | |

---

### 9. Quests (Muse’s Companion)
| Field | Type | Notes |
|-------|------|-------|
| quest_id | UUID (PK) | |
| user_id | UUID (FK) | |
| type | Enum | (cross-medium, random, project-linked) |
| description | Text | |
| linked_ideas | Array | Associated ideas |
| status | Enum | (open, complete, skipped) |
| created_at | Timestamp | |

---

## 🔗 Entity-Relationship Summary

```
User 1---* Ideas
User 1---* Projects
User 1---* Reflections
User 1---* Materials
User 1---* Quests

Project *---* Ideas   (via Project-Idea Links)
Project 1---* Lore
Project 1---* Mechanics

Idea *---* Reflections (optional link)
Idea *---* Quests (optional link)
Lore *---* Ideas (optional link)
Mechanics *---* Ideas (optional link)
```

---

## 📊 Entity-Relationship Diagram (ERD)

![Ikigai App ERD](sandbox:/mnt/data/ikigai_app_erd.png)

---

## ⚖️ Development Phases

**Phase 1: Core Foundations**
- Implement Users, Ideas, Projects, Reflections.
- Build Forge (capture) + Compass (reflection).

**Phase 2: Creative Tools**
- Expand with Story Loom (Lore) and Maker’s Ledger (Materials).

**Phase 3: Playful Expansion**
- Add Muse’s Companion (Quests) + Gamecrafter’s Anvil (Mechanics).

**Phase 4: Integration**
- Cross-link modules, shared dashboards, AI assist.

---

## 🏗 Guiding Principle

All six relics (apps) share a **unified schema**. Data is never siloed: every spark of inspiration (Idea) can find a home in multiple projects, cross mediums, and feed into daily reflection. This ensures the constellation works as **one living ecosystem**, not six separate tools.

