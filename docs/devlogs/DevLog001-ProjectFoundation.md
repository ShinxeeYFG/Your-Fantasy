# Dev Log #001 - Project Foundation

**Date:** July 3, 2026

## Objective

Today's goal was to begin laying the foundation for *Your Fantasy* by creating the first playable prototype environment while also establishing professional software development workflows for the project.

Although the visible progress appears small, today was spent learning new tools, establishing project standards, and building the infrastructure that future development will rely on.

---

# Unreal Engine Progress

## Starter Town Prototype

Created the first prototype multiplayer landing area that will eventually become the game's initial spawn location.

### Accomplishments

- Imported modular medieval building assets
- Learned Unreal Engine 5 material editor
- Worked with:
  - Base Color textures
  - Normal Maps
  - ORM (Occlusion / Roughness / Metallic) textures
- Learned how texture tiling works
- Experimented with World Aligned Materials
- Learned limitations of world-aligned textures on flat modular meshes
- Painted foliage around the town
- Built a simple forest perimeter
- Created the first cobblestone town layout
- Experimented with modeling mode to create large walkable surfaces



### Lessons Learned

One of the biggest challenges today involved understanding how Unreal handles texture coordinates versus world-aligned textures.

Although the final solution was much simpler than expected, the troubleshooting process greatly improved my understanding of Unreal's material system.

---

# Project Management

Today I also began organizing the project using GitHub as if it were being developed by a small software studio.

## Repository Created

Repository Name:

YourFantasy

Created initial README documenting:

- Project vision
- Core philosophy
- Design goals

---

# Git Workflow

Today I learned professional Git workflows including:

- Repository structure
- Branching strategy
- Pull Requests
- Code Reviews
- Merge workflow
- Branch cleanup

Current workflow:

Main
↓
Feature Branch
↓
Commit
↓
Push
↓
Pull Request
↓
Review
↓
Merge
↓
Delete Branch

One important lesson learned today was that Git tracks changes rather than folders. Existing folders are merged automatically without duplication, while only modified files require review.

---

# Documentation Structure

Established the documentation layout for the project.

Current structure:

docs/

- philosophy/
- architecture/
- devlogs/
- api/
- tsg/

Created initial documentation including:

- Design Philosophy
- System Architecture

---

# GitHub Project Organization

Set up project planning using GitHub Projects.

Current organization:

Road Map

- Long-term project chapters
- Major milestones

Sprint Board

- Weekly development tasks
- Active work
- Completed work

This separates long-term vision from short-term development goals.

---

# DevOps Planning

Planned the first backend milestone.

Upcoming work:

- Azure/Linux VM
- Dedicated server hosting
- Backend API
- Database
- Login UI
- Account creation
- CI/CD deployment pipeline

The long-term goal is to build production-style infrastructure that mirrors real software engineering practices while supporting multiplayer gameplay.

---

# Philosophy

Today's biggest realization wasn't about Unreal—it was about the project itself.

I don't simply want to build an MMORPG.

I want to build it the same way a professional software team would.

This means documenting decisions, maintaining source control, planning work through sprints, using pull requests, deploying backend services, and creating repeatable development workflows.

By the time *Your Fantasy* becomes playable, the project itself should demonstrate growth in game development, backend engineering, cloud infrastructure, DevOps, and software architecture.

---

# Next Objectives

- Finish project documentation
- Design backend architecture
- Provision first VM
- Create authentication API
- Connect Unreal login UI to backend
- Store first player account in the database
- Successfully log into the game from a packaged client
