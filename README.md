<div align="center">

# Laravel AI Support Tool

**Multi-tenant customer support platform with Claude AI
response drafting — built to demonstrate Laravel + AI integration.**

[![Laravel](https://img.shields.io/badge/Laravel-11.x-FF2D20?style=flat&logo=laravel&logoColor=white)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-8.2+-777BB4?style=flat&logo=php&logoColor=white)](https://php.net)
[![Claude API](https://img.shields.io/badge/Claude-API-7C3AED?style=flat)](https://anthropic.com)
[![Filament](https://img.shields.io/badge/Filament-3.x-F59E0B?style=flat)](https://filamentphp.com)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

[Live Demo](https://demo.yourname.dev/support) ·
[Loom Walkthrough](https://loom.com/your-link) ·
[Case Study](https://yourname.dev/projects/ai-support-tool)

</div>

---

## What This Is

A production-quality, multi-tenant customer support platform
that uses the Claude API to draft responses to incoming tickets.
Built as a portfolio project to demonstrate:

- **Multi-tenancy** with `stancl/tenancy`
- **AI integration** (streaming Claude responses into the admin UI)
- **Filament admin panel** for ticket management
- **Real-time notifications** via Slack webhook
- **Role-based access control** (admin, agent, viewer)

## Architecture

## Key Features

- ✅ Multi-tenant — each client gets isolated data
- ✅ AI drafting — Claude auto-drafts responses based on ticket content
- ✅ Streaming — responses stream in real-time (no waiting)
- ✅ Ticket management — status, priority, SLA tracking, tagging
- ✅ Slack integration — new tickets trigger a Slack webhook
- ✅ RBAC — admin, agent, viewer roles per tenant
- ✅ Cost control — token usage tracked per tenant, per month

## Tech Stack

| | |
|---|---|
| Framework | Laravel 11 |
| Multi-tenancy | stancl/tenancy |
| Admin panel | Filament 3 + Livewire 3 |
| AI | Anthropic Claude API (claude-3-5-sonnet) |
| Database | MySQL 8 (one DB per tenant) |
| Queue | Laravel Horizon (Redis) |
| Auth | Laravel Sanctum |
| Notifications | Slack Webhook, Laravel Mail |

## Setup

```bash
git clone https://github.com/shafquatimam/laravel-ai-support-tool.git
cd laravel-ai-support-tool

composer install && npm install

cp .env.example .env
php artisan key:generate

# Add your credentials to .env:
# ANTHROPIC_API_KEY=sk-ant-...
# SLACK_WEBHOOK_URL=https://hooks.slack.com/...

php artisan migrate
php artisan tenants:migrate
php artisan db:seed

npm run dev && php artisan serve
```

**Demo credentials:**
- Admin: `admin@demo.com` / `password`
- Agent: `agent@demo.com` / `password`

## AI Integration Detail

```php
// ClaudeService.php
public function draftResponse(Ticket $ticket): string
{
    return $this->client->messages()->create([
        'model'      => 'claude-3-5-sonnet-20241022',
        'max_tokens' => 500,
        'system'     => $this->buildSystemPrompt($ticket->tenant),
        'messages'   => [
            ['role' => 'user', 'content' => $ticket->body]
        ],
    ])->content[0]->text;
}
```

## Project Context

Built in 7 days as Sprint 2 of a 90-day freelance portfolio
project. See the full case study:
[yourname.dev/projects/ai-support-tool](https://yourname.dev/projects/ai-support-tool)

---

**Built by [Shafquat Imam](https://yourname.dev)**
Senior Laravel Developer · Available for freelance
