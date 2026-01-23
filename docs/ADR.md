# Architecture Decision Records (ADR)

Este documento registra as principais decisões arquiteturais tomadas no projeto de geração de currículo.

---

## ADR-001: Adoção do JSON Resume como Schema Base

**Data:** 2024-01-01
**Status:** Aceito
**Contexto:** Necessidade de um formato estruturado e padronizado para armazenar dados de currículo que seja interoperável com outras ferramentas.

**Decisão:** Adotar o padrão [JSON Resume v1.0.0](https://jsonresume.org/schema/) como base do schema, com extensões customizadas para suportar funcionalidades adicionais.

**Consequências:**
- (+) Compatibilidade com ecossistema existente de ferramentas JSON Resume
- (+) Schema bem documentado e validado pela comunidade
- (+) Facilita exportação para múltiplos formatos
- (-) Necessidade de extensões para atender requisitos específicos (ATS, i18n)

**Alternativas Consideradas:**
1. Schema proprietário: descartado por falta de interoperabilidade
2. LinkedIn Export: descartado por limitações de estrutura e privacidade
3. Europass XML: descartado por complexidade excessiva

---

## ADR-002: Extensões Customizadas com Prefixo x-

**Data:** 2024-01-01
**Status:** Aceito
**Contexto:** O schema JSON Resume padrão não suporta dados específicos para ATS, internacionalização e metadados customizados.

**Decisão:** Criar extensões usando prefixo `x-` seguindo convenção de propriedades de extensão:
- `x-atsData`: Dados otimizados para sistemas ATS
- `x-custom`: Objetivos e timeline customizados
- `x-integrations`: Integrações com plataformas externas
- `x-i18n`: Traduções para múltiplos idiomas

**Consequências:**
- (+) Separação clara entre dados padrão e customizados
- (+) Validação independente possível
- (+) Não quebra compatibilidade com ferramentas JSON Resume
- (-) Aumenta complexidade do schema

---

## ADR-003: Armazenamento de Traduções no Próprio JSON (i18n)

**Data:** 2024-01-01
**Status:** Aceito
**Contexto:** Necessidade de manter o currículo em múltiplos idiomas (pt-BR, en-US).

**Decisão:** Armazenar traduções dentro do próprio arquivo JSON usando a extensão `x-i18n`, referenciando itens pelo seu `id`.

**Estrutura:**
```json
{
  "work": [{ "id": "work-001", "position": "Analista Senior", ... }],
  "x-i18n": {
    "en-US": {
      "work": {
        "work-001": { "position": "Senior Analyst", ... }
      }
    }
  }
}
```

**Consequências:**
- (+) Single source of truth - um único arquivo
- (+) Fácil sincronização entre idiomas
- (+) Versionamento simplificado
- (-) Arquivo JSON mais extenso
- (-) Requer IDs únicos em todos os itens traduzíveis

**Alternativas Consideradas:**
1. Arquivos separados por idioma (`cv_pt-BR.json`, `cv_en-US.json`): descartado por duplicação de dados não traduzíveis
2. Sistema de chaves de tradução externo: descartado por complexidade

---

## ADR-004: Validação em Duas Camadas (Schema + Estrutura)

**Data:** 2024-02-01
**Status:** Aceito
**Contexto:** Garantir integridade dos dados do CV antes de commits e exportações.

**Decisão:** Implementar validação em duas camadas:
1. **Validação de Schema (AJV):** Verifica tipos, formatos e constraints do JSON Schema
2. **Validação de Estrutura (Python):** Verifica regras de negócio e consistência lógica

**Consequências:**
- (+) Validação robusta e abrangente
- (+) Mensagens de erro específicas por camada
- (+) Reutilização de ferramentas padrão (ajv-cli)
- (-) Duas dependências de runtime diferentes (Node.js, Python)

---

## ADR-005: Exportação via Templates HTML/Jinja2

**Data:** 2024-03-01
**Status:** Aceito
**Contexto:** Necessidade de exportar o CV para formatos PDF e DOCX com layout profissional e compatível com ATS.

**Decisão:** Usar templates HTML com Jinja2 para renderização, convertendo para PDF via xhtml2pdf e DOCX via python-docx.

**Consequências:**
- (+) Templates fáceis de customizar (HTML/CSS)
- (+) Suporte a múltiplos templates (ATS, modern)
- (+) Controle total sobre layout
- (-) Dependências Python adicionais
- (-) Conversão HTML→PDF pode ter limitações de CSS

**Alternativas Consideradas:**
1. LaTeX: descartado por complexidade de manutenção
2. Microsoft Word como base: descartado por dificuldade de versionamento
3. WeasyPrint: opcional, requer GTK3 no Windows

---

## ADR-006: GitHub Actions para CI/CD

**Data:** 2024-04-01
**Status:** Aceito
**Contexto:** Automatizar validação do CV em cada commit e pull request.

**Decisão:** Usar GitHub Actions com workflow de validação automática que:
1. Valida schema JSON
2. Valida estrutura lógica
3. Cria issue automaticamente em caso de falha

**Consequências:**
- (+) Validação automática em cada push
- (+) Feedback imediato via issues do GitHub
- (+) Integração nativa com o repositório
- (-) Dependência do GitHub como plataforma

---

## ADR-007: Versionamento Semântico para o CV

**Data:** 2024-05-01
**Status:** Aceito
**Contexto:** Necessidade de rastrear mudanças significativas no CV ao longo do tempo.

**Decisão:** Adotar versionamento semântico (MAJOR.MINOR.PATCH) no campo `meta.version`:
- **MAJOR:** Mudanças estruturais ou de carreira significativas
- **MINOR:** Novas experiências, certificações ou skills
- **PATCH:** Correções, ajustes de texto, melhorias menores

**Consequências:**
- (+) Histórico claro de evolução do CV
- (+) Facilita rollback se necessário
- (+) Comunicação clara do tipo de mudança
- (-) Requer disciplina para manter

---

## ADR-008: Otimização para ATS (Applicant Tracking Systems)

**Data:** 2024-06-01
**Status:** Aceito
**Contexto:** Currículos modernos precisam ser parseáveis por sistemas ATS como Workday, Greenhouse, Lever, etc.

**Decisão:** Implementar extensão `x-atsData` com:
- Keywords categorizadas (técnicas, soft skills, metodologias, indústrias)
- Conquistas quantificadas
- Score de compatibilidade ATS
- Campos otimizados para parsing

**Consequências:**
- (+) Maior visibilidade em processos seletivos automatizados
- (+) Keywords estruturadas para matching
- (+) Métricas de qualidade do CV
- (-) Manutenção adicional das keywords

---

## ADR-009: Nomenclatura Padronizada de Arquivos de Saída

**Data:** 2024-07-01
**Status:** Aceito
**Contexto:** Necessidade de identificar facilmente arquivos exportados e evitar sobrescrita acidental.

**Decisão:** Adotar padrão de nomenclatura:
```
CV - {Nome} ({Data}) - {Idioma}.{extensão}
Exemplo: CV - Ed Costa (2026-01-22) - pt-BR.pdf
```

Para arquivos duplicados, adicionar sufixo incremental:
```
CV - Ed Costa (2026-01-22) - pt-BR (1).pdf
```

**Consequências:**
- (+) Identificação clara do conteúdo e versão
- (+) Evita sobrescrita acidental
- (+) Facilita organização de múltiplas versões
- (-) Nomes de arquivo mais longos

---

## ADR-010: IDs Únicos por Entidade

**Data:** 2024-08-01
**Status:** Aceito
**Contexto:** Necessidade de referenciar itens específicos do CV para traduções (i18n) e atualizações.

**Decisão:** Cada entidade principal (work, education, awards, certificates, publications) deve ter um campo `id` único seguindo o padrão:
- `work-XXX` para experiências profissionais
- `edu-XXX` para educação
- `award-XXX` para premiações
- `cert-XXX` para certificações
- `pub-XXX` para publicações

**Consequências:**
- (+) Referenciamento preciso para traduções
- (+) Facilita merges e atualizações parciais
- (+) Suporta ordenação customizada
- (-) Gestão manual de IDs

---

## Decisões Futuras (Backlog)

| ID | Título | Status |
|----|--------|--------|
| ADR-011 | Integração com LinkedIn API | Pendente |
| ADR-012 | Geração automática de cover letter | Em análise |
| ADR-013 | Customização por vaga (job targeting) | Em análise |
| ADR-014 | Web app para edição visual | Pendente |
| ADR-015 | Histórico de versões com diff visual | Pendente |

---

## Template para Novas ADRs

```markdown
## ADR-XXX: [Título da Decisão]

**Data:** YYYY-MM-DD
**Status:** Proposto | Aceito | Depreciado | Substituído por ADR-XXX
**Contexto:** [Descrição do problema ou necessidade]

**Decisão:** [Descrição da decisão tomada]

**Consequências:**
- (+) [Benefício 1]
- (+) [Benefício 2]
- (-) [Desvantagem 1]
- (-) [Desvantagem 2]

**Alternativas Consideradas:**
1. [Alternativa 1]: [motivo de descarte]
2. [Alternativa 2]: [motivo de descarte]
```
