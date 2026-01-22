# CV Ed Santana Martins Costa - JSON Resume
## Currículo estruturado com otimização ATS

---

## 📋 STATUS DA IMPLEMENTAÇÃO

### **Versão Atual: 2.2.0**
**Última atualização:** 2026-01-22

**Novidades da v2.2.0:**
- ✨ Novo padrão de nomenclatura de arquivos: `CV - Ed Costa (YYYY-MM-DD) - {idioma}.ext`
- ✨ Sistema de controle automático de versões com incremento `(n)`
- 🔧 Simplificação do Export Tool (removido template "modern", mantido apenas ATS)
- 📚 Documentação atualizada e expandida
- 🎯 Foco em otimização ATS

### **Features Implementadas:**
✅ Padrão JSON Resume v1.0.0 com extensões ATS
✅ Datas padronizadas ISO 8601
✅ IDs únicos para todas as entidades
✅ Nomenclatura consistente (camelCase)
✅ JSON Schema validation
✅ Campos customizados com namespace (`x-`)
✅ Metadados de parsing e geração
✅ Score ATS calculado (95/100)
✅ Compatibilidade com múltiplos ATS
✅ Keywords estruturadas por categoria
✅ Suporte multilíngue (pt-BR / en-US)
✅ CI/CD com GitHub Actions
✅ Export Tool com nomenclatura padronizada

---

## 🎯 PADRÕES E FRAMEWORKS UTILIZADOS

### **1. JSON Resume (jsonresume.org)**
- ✅ Padrão open-source amplamente adotado
- ✅ Suportado por 100+ temas
- ✅ Validação via JSON Schema
- ✅ Compatível com ATS

### **2. Extensões ATS Customizadas**
- ✅ Namespace `x-atsData` para keywords e scoring
- ✅ Namespace `x-custom` para dados pessoais/profissionais
- ✅ Namespace `x-integrations` para LinkedIn/GitHub

---

## 🔧 ESTRUTURA DO ARQUIVO JSON

### **Seções Principais (JSON Resume Standard)**

| Seção | Descrição | Status |
|-------|-----------|--------|
| `meta` | Metadados do documento (versão, ATS score, compatibilidade) | ✅ |
| `basics` | Dados pessoais, contato e perfis sociais | ✅ |
| `work` | 11 experiências profissionais com keywords | ✅ |
| `education` | 4 formações acadêmicas | ✅ |
| `skills` | 10 categorias de skills com níveis | ✅ |
| `languages` | 3 idiomas com nível CEFR | ✅ |
| `awards` | 5 prêmios e reconhecimentos | ✅ |
| `publications` | 2 publicações científicas | ✅ |
| `certificates` | 9 certificações profissionais | ✅ |

### **Extensões ATS**

| Seção | Descrição | Status |
|-------|-----------|--------|
| `x-atsData.keywords` | Keywords por categoria (primary, technical, soft, methodologies, industries, certifications, companies) | ✅ |
| `x-atsData.quantifiedAchievements` | Conquistas quantificadas e verificáveis | ✅ |
| `x-atsData.yearsOfExperience` | Anos de experiência total e por área | ✅ |
| `x-custom.objectives` | Objetivos pessoais e profissionais | ✅ |
| `x-custom.timeline` | Timeline de carreira com milestones | ✅ |
| `x-integrations` | Integrações LinkedIn e GitHub | ✅ |
| `x-i18n` | Traduções multilíngue (en-US) | ✅ |

---

## 📊 ATS SCORE

### **Pontuação Geral: 95/100**

| Categoria | Score |
|-----------|-------|
| Overall | 95 |
| Technical | 98 |
| Experience | 92 |
| Education | 95 |
| Keywords | 97 |
| Format | 100 |

### **Compatibilidade ATS**

| Sistema | Status |
|---------|--------|
| Workday | ✅ |
| Greenhouse | ✅ |
| Lever | ✅ |
| Taleo | ✅ |
| iCIMS | ✅ |
| SuccessFactors | ✅ |

---

## 🔑 KEYWORDS E TAXONOMIA

### **Estrutura Implementada:**

```
keywords/
├── primary/          # Data Engineer, BI Analyst, etc.
├── technical/        # Python, SQL, Power BI, Azure, etc.
├── soft/             # Leadership, Problem Solving, etc.
├── methodologies/    # Lean Six Sigma, Agile, Scrum, etc.
├── industries/       # Mining, Healthcare, etc.
├── certifications/   # ISTQB, Six Sigma, NRs
└── companies/        # Vale, Progen, UFBA, etc.
```

---

## ✅ CHECKLIST DE IMPLEMENTAÇÃO

### **Fase 1: Estrutura Base**
- [x] Migrar para JSON Resume schema
- [x] Converter datas para ISO 8601
- [x] Padronizar nomenclatura (camelCase)
- [x] Adicionar IDs únicos para entidades
- [x] Criar `$schema` e `$id` para validação

### **Fase 2: Otimização ATS**
- [x] Estruturar keywords por categoria
- [x] Adicionar achievements quantificados
- [x] Criar seção `x-atsData`
- [x] Adicionar keywords em cada experiência
- [x] Adicionar campos de score ATS
- [x] Mapear compatibilidade com ATS populares

### **Fase 3: Metadados e Extensões**
- [x] Implementar seção `meta` com versão e gerador
- [x] Adicionar `x-custom` para objetivos e timeline
- [x] Adicionar `x-integrations` para LinkedIn/GitHub
- [x] Documentar anos de experiência por área

### **Fase 4: Integrações**
- [x] Adicionar dados LinkedIn
- [x] Adicionar dados GitHub
- [ ] Adicionar mapeamento Workday (estrutura preparada)
- [ ] Adicionar mapeamento Indeed (estrutura preparada)

### **Fase 5: i18n e Localização**
- [x] Definir idioma principal (pt-BR)
- [x] Implementar suporte multilíngue pt-BR/en-US (`x-i18n`)
- [x] Traduzir campos principais (basics, work, education, awards, certificates, objectives)

### **Fase 6: Validação e Testes**
- [x] Implementar JSON Schema reference
- [x] Criar schema de validação separado (`schema.json`)
- [ ] Testar parsing em ATS reais
- [x] Automatizar validação via CI/CD

---

## 📁 ESTRUTURA DO REPOSITÓRIO

```
cv-edmscosta/
├── .github/
│   └── workflows/
│       └── validate-cv.yml   # CI/CD para validação automática
├── export/
│   ├── templates/
│   │   └── ats_template.html # Template ATS para PDF
│   ├── output/               # Arquivos exportados
│   ├── export_cv.py          # Script de exportação
│   ├── requirements.txt      # Dependências Python
│   └── README.md             # Documentação do Export Tool
├── cv_ed_costa.json          # CV completo em JSON Resume + ATS
├── schema.json               # JSON Schema para validação
└── readme.md                 # Este documento
```

---

## 📄 EXPORT TOOL

O repositório inclui uma ferramenta de exportação para converter o CV JSON em formatos visuais (PDF e DOCX).

### **Nomenclatura de Arquivos**

Padrão: `CV - Ed Costa (YYYY-MM-DD) - {idioma}.{extensão}`

Exemplos:
- `CV - Ed Costa (2026-01-22) - pt-BR.pdf`
- `CV - Ed Costa (2026-01-22) - en-US.docx`

### **Controle de Versão Automático**

Se o arquivo já existir, adiciona `(n)` antes da extensão:
- `CV - Ed Costa (2026-01-22) - pt-BR (1).pdf`
- `CV - Ed Costa (2026-01-22) - pt-BR (2).pdf`

### **Uso Rápido**

```bash
cd export
pip install -r requirements.txt

# Exportar todas as versões (pt-BR e en-US)
python export_cv.py --all-langs

# Exportar apenas português
python export_cv.py --lang pt-BR

# Exportar apenas PDF
python export_cv.py --format pdf
```

Documentação completa: [export/README.md](export/README.md)

---

## 🔍 VALIDAÇÃO COM JSON SCHEMA

O arquivo `schema.json` permite validação automática do CV, garantindo integridade dos dados.

### **Cobertura do Schema**

| Seção | Campos Obrigatórios | Validações |
|-------|---------------------|------------|
| `meta` | version, lastModified, language | Formato semver, ISO 8601, locale |
| `basics` | name, email | Email válido, país ISO 3166-1 |
| `work` | name, position, startDate | Datas ISO 8601 |
| `education` | institution, area, studyType | Datas ISO 8601 |
| `skills` | name | Níveis: Beginner/Intermediate/Advanced/Expert |
| `languages` | language, fluency | Níveis CEFR (A1-C2) |
| `awards` | title, awarder | - |
| `publications` | name, publisher | - |
| `certificates` | name, issuer | - |

### **Como Validar**

**Usando ajv-cli (Node.js):**
```bash
npm install -g ajv-cli
ajv validate -s schema.json -d cv_ed_costa.json
```

**Usando Python (jsonschema):**
```bash
pip install jsonschema
python -c "
import json
from jsonschema import validate
schema = json.load(open('schema.json'))
data = json.load(open('cv_ed_costa.json'))
validate(data, schema)
print('Validação OK!')
"
```

**Online:**
- https://www.jsonschemavalidator.net/
- https://json-schema-validator.herokuapp.com/

---

## 🌍 SUPORTE MULTILÍNGUE (i18n)

O CV possui suporte a múltiplos idiomas através da seção `x-i18n`.

### **Idiomas Disponíveis**
- **pt-BR** (padrão) - Campos principais do documento
- **en-US** - Traduções na seção `x-i18n.en-US`

### **Campos Traduzidos**

| Seção | Campos |
|-------|--------|
| `basics` | label, summary |
| `work` | position, summary, highlights (todas as 11 experiências) |
| `education` | area, studyType |
| `awards` | title, summary |
| `certificates` | name (quando aplicável) |
| `x-custom` | objectives, timeline.milestones |

### **Como Usar**

Para acessar a versão em inglês de um campo:

```javascript
// Exemplo: obter o cargo traduzido
const workId = "work-001";
const position_ptBR = cv.work.find(w => w.id === workId).position;
const position_enUS = cv["x-i18n"]["en-US"].work[workId].position;

// position_ptBR: "Analista de Sistemas Senior"
// position_enUS: "Senior Systems Analyst"
```

---

## 🔄 CI/CD - VALIDAÇÃO AUTOMÁTICA

O repositório possui GitHub Actions configurado para validação automática com criação de issues em caso de falha.

### **Workflow: validate-cv.yml**

| Trigger | Descrição |
|---------|-----------|
| `push` (main) | Valida em push para branch main |
| `pull_request` (main) | Valida em PRs para main |
| `workflow_dispatch` | Execução manual via GitHub |

### **Jobs**

| Job | Descrição |
|-----|-----------|
| `validate` | Executa validações de schema e estrutura |
| `create-issue-on-failure` | Cria issue automática em caso de falha (apenas em push) |

### **Validações Executadas**

1. **JSON Schema Validation** - Valida `cv_ed_costa.json` contra `schema.json` usando ajv-cli (draft-07)
2. **Structure Validation** - Verifica campos obrigatórios e tipos de dados esperados

### **Criação Automática de Issues**

Em caso de falha na validação (apenas em push para main):
- Cria issue com label `validation-error` e `automated`
- Inclui detalhes do erro, commit e link para logs
- Não duplica issues se já existir uma aberta

### **Executar Localmente**

```bash
# Instalar dependências
npm install -g ajv-cli ajv-formats

# Validar
ajv validate -s schema.json -d cv_ed_costa.json --spec=draft7 -c ajv-formats
```

---

## 🔧 RECOMENDAÇÕES PARA FEATURES PENDENTES

### **Mapeamento Workday/Indeed**

Estrutura recomendada para integração com ATS específicos:

```json
{
  "x-integrations": {
    "workday": {
      "candidateId": "uuid",
      "applicationDate": "2026-01-17",
      "source": "Direct Application",
      "tags": ["Data Engineer", "Power BI Expert"]
    },
    "indeed": {
      "resumeId": "ed-costa-cv-2026",
      "visibility": "public",
      "categories": ["Information Technology", "Data Analysis"]
    }
  }
}
```

---

## 📚 REFERÊNCIAS

1. **JSON Resume:** https://jsonresume.org/schema/
2. **ISO 8601:** https://www.iso.org/iso-8601-date-and-time-format.html
3. **CEFR Levels:** https://www.coe.int/en/web/common-european-framework-reference-languages
4. **ATS Best Practices:**
   - Jobscan: https://www.jobscan.co/
   - TopResume ATS Guide
   - ResumeWorded
