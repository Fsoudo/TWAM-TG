# Escapadinhas — Plataforma Web de Planeamento de Férias

> Aplicação web que centraliza a consulta meteorológica, gestão de alojamentos e organização de planos de férias numa única plataforma.

---

## Sobre o Projeto

O **Escapadinhas** surgiu da necessidade de eliminar a dependência de múltiplas ferramentas isoladas (AccuWeather, Airbnb, notas do telemóvel) no processo de planeamento de férias. A plataforma reúne tudo num só lugar:

- Previsão meteorológica por destino e datas
- Catálogo de alojamentos gerido pelos próprios utilizadores
- Planos de férias pessoais com estado e histórico

Projeto desenvolvido no âmbito da UC **Tecnologias Web e Aplicações Móveis (TWAM)** — Instituto Politécnico de Beja, 2025/2026.

---

## Funcionalidades

### Utilizador Planeador
| Funcionalidade | Método | Endpoint |
|----------------|--------|----------|
| Consultar previsão meteorológica | GET | Open-Meteo API (externa) |
| Listar alojamentos disponíveis | GET | `/imoveis` |
| Guardar novo plano de férias | POST | `/planos` |
| Editar plano de férias | PATCH | `/planos/{id}` |
| Eliminar plano de férias | DELETE | `/planos/{id}` |

### Utilizador Gestor de Propriedades
| Funcionalidade | Método | Endpoint |
|----------------|--------|----------|
| Adicionar novo imóvel | POST | `/imoveis` |
| Editar imóvel existente | PATCH | `/imoveis/{id}` |
| Remover imóvel | DELETE | `/imoveis/{id}` |

### Qualquer Visitante
- Consultar a landing page e informação geral da plataforma
- Registar nova conta
- Fazer login

---

## Tecnologias

| Camada | Tecnologia |
|--------|------------|
| Frontend | HTML5 · CSS3 · JavaScript (Vanilla) |
| API Própria | [JSON Server](https://github.com/typicode/json-server) |
| Meteorologia | [Open-Meteo API](https://open-meteo.com) |
| Geocodificação | Open-Meteo Geocoding API |
| Design | Interface mobile-first, responsiva |

---

## Arquitetura

```
[Utilizador Não Autenticado]
        │
        ▼
  Login / Registo  ──►  POST /utilizadores
        │
        ├──────────────────────────────────────────────────────────┐
        ▼                                                          ▼
[Utilizador Planeador]                          [Utilizador Gestor de Propriedades]
        │                                                          │
        ├──► Consultar Meteorologia                                ├──► Adicionar Imóvel  ──► POST /imoveis
        │    └──► GET api.open-meteo.com                          │
        │                                                          ├──► Editar Imóvel  ──► PATCH /imoveis/{id}
        ├──► Visualizar Imóveis ◄──── GET /imoveis ◄──────────────┘
        │
        ├──► Guardar Plano de Férias  ──► POST /planos
        │
        └──► Editar / Eliminar Plano  ──► PATCH / DELETE /planos/{id}
```

---

## Estrutura da API Própria (JSON Server)

```json
{
  "utilizadores": [],
  "imoveis": [],
  "planos": []
}
```

**Base URL (desenvolvimento):** `http://localhost:3000`

| Recurso | URL |
|---------|-----|
| Utilizadores | `/utilizadores` |
| Imóveis | `/imoveis` |
| Planos de Férias | `/planos` |

---

## Como Executar

### Pré-requisitos
- [Node.js](https://nodejs.org) instalado
- [JSON Server](https://github.com/typicode/json-server) (`npm install -g json-server`)

### Passos

```bash
# 1. Clonar o repositório
git clone https://github.com/<utilizador>/escapadinhas.git
cd escapadinhas

# 2. Iniciar a API local
json-server --watch db.json --port 3000

# 3. Abrir o frontend no browser
# Abrir index.html diretamente no browser ou usar um servidor local (ex: Live Server no VS Code)
```

A API ficará disponível em `http://localhost:3000`.

---

## Perfis de Utilizador

| Perfil | Acesso |
|--------|--------|
| **Visitante** | Landing page, registo, login |
| **Planeador** | Meteorologia, lista de imóveis, gestão dos seus planos |
| **Gestor de Propriedades** | Todas as funcionalidades do Planeador + gestão de imóveis |

---

## APIs Externas

### Open-Meteo — Previsão Meteorológica
```
GET https://api.open-meteo.com/v1/forecast
    ?latitude={lat}&longitude={lon}
    &daily=temperature_2m_max,temperature_2m_min,precipitation_sum,windspeed_10m_max
    &start_date={YYYY-MM-DD}&end_date={YYYY-MM-DD}
    &timezone=auto
```

### Open-Meteo — Geocodificação (cidade → coordenadas)
```
GET https://geocoding-api.open-meteo.com/v1/search?name={cidade}&count=1&language=pt
```

Ambas as APIs são **gratuitas e sem necessidade de registo ou chave**.

---

## Protótipo de Usabilidade

O projeto inclui um protótipo não funcional interativo (`apresentacao_teste.html`) criado para sessões de teste de usabilidade com utilizadores reais, seguindo a metodologia **Think Aloud** e avaliação **SUS (System Usability Scale)**.

Os testes foram realizados com 5 participantes, cobrindo 6 tarefas:

1. Criação de conta (registo)
2. Consulta meteorológica
3. Guardar plano de férias
4. Consultar planos guardados
5. Adicionar imóvel
6. Navegação de regresso à página inicial

---

## Autores

| Nome | Número |
|------|--------|
| Francisco Soudo| 14060 |
| Miguel Pauzinho | 27131 |

---

## Contexto Académico

| Campo | Detalhe |
|-------|---------|
| **Instituição** | Instituto Politécnico de Beja — ESTIG |
| **Curso** | Engenharia Informática |
| **UC** | Tecnologias Web e Aplicações Móveis (TWAM) |
| **Ano Letivo** | 2025/2026 |
