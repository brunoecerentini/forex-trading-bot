# Sistema de Trading Automatizado - Bot Forex & Ouro

> **Nota**: Esta é a versão em português do README. Para a versão em inglês (recomendada para GitHub público), veja [README.md](README.md)

## 🎯 Visão Geral

Sistema de trading algorítmico profissional que implementa estratégias baseadas em zonas de suporte e resistência para múltiplos pares de moedas Forex e Ouro (XAUUSD). O bot usa análise técnica para identificar oportunidades de trading e executa operações automaticamente via MetaTrader 5.

## ✨ Características Principais

- **Suporte Multi-Moedas**: XAUUSD, EURUSD, GBPUSD, USDJPY
- **Execução Automática**: Ordens de compra/venda automáticas via MT5
- **Gestão de Risco**: Stop-loss e take-profit automáticos
- **Análise em Tempo Real**: Monitoramento contínuo do mercado
- **Integração Azure ML**: Treinamento de modelos na nuvem
- **Código Production-Ready**: Variáveis de ambiente, tratamento de erros, logs

## 🚀 Início Rápido

### 1. Instalar Dependências

```bash
# Criar ambiente virtual
python -m venv venv
venv\Scripts\activate

# Instalar pacotes
pip install -r requirements.txt
```

### 2. Configurar Credenciais

Copie `.env.example` para `.env` e preencha com suas credenciais:

```env
MT5_ACCOUNT=seu_numero_conta
MT5_PASSWORD=sua_senha
MT5_SERVER=servidor_broker
LOT_SIZE=0.01
```

### 3. Executar o Bot

**Jupyter Notebook** (Recomendado):
```bash
jupyter notebook
# Abra BEST----XAUUSD.IPYNB e execute todas as células
```

**Script Python**:
```bash
python codex.py --symbol XAUUSD
```

## 📊 Estratégia de Trading

### Detecção de Zonas

1. **CR (Central Range)**: Máximo e mínimo dos primeiros 10 candles
2. **ZN (Zone)**: Extensão acima/abaixo de CR pela distância do range
3. **N1, N2, N3**: Níveis de extensão para take-profit

### Sinais de Entrada

**Sinal de COMPRA**:
- Preço abre abaixo de ZN_UPPER
- Preço fecha acima de ZN_UPPER
- Stop Loss: CR_BOT - spread
- Take Profit: N1_100_UPPER

**Sinal de VENDA**:
- Preço abre acima de ZN_BOT
- Preço fecha abaixo de ZN_BOT
- Stop Loss: CR_UPPER + spread
- Take Profit: N1_100_BOT

## 📁 Estrutura do Projeto

```
forex-trading-bot/
├── BEST----XAUUSD.IPYNB      # Bot Ouro (SANITIZADO)
├── BEST----EURUSD.IPYNB      # Bot EUR/USD (SANITIZADO)
├── BEST----GBPUSD.IPYNB      # Bot GBP/USD (SANITIZADO)
├── BEST----USDJPY.IPYNB      # Bot USD/JPY (SANITIZADO)
├── codex.py                   # Interface CLI
├── train.py                   # Treino de modelos ML
├── generate_dataset.py        # Geração de datasets
├── RFE_feature_selection.py   # Seleção de features
├── AzureML_artefatos.ipynb   # Integração Azure ML
├── .env.example               # Template de variáveis
├── .gitignore                 # Exclusões Git
└── requirements.txt           # Dependências Python
```

## 🛡️ Segurança

**⚠️ IMPORTANTE**: Nunca commitar informações sensíveis!

- ✅ Use variáveis de ambiente (.env)
- ✅ .env está no .gitignore
- ✅ Revise código antes de push
- ❌ Nunca hardcode senhas
- ❌ Nunca commita dados de trading reais

Veja [SECURITY.md](SECURITY.md) para mais detalhes.

## 📖 Documentação Completa

- **[README.md](README.md)**: Documentação principal (inglês)
- **[USAGE.md](USAGE.md)**: Guia detalhado de uso
- **[QUICK_START.md](QUICK_START.md)**: Guia de início rápido
- **[SECURITY.md](SECURITY.md)**: Práticas de segurança
- **[PORTFOLIO_OVERVIEW.md](PORTFOLIO_OVERVIEW.md)**: Para recrutadores
- **[SETUP_INSTRUCTIONS.md](SETUP_INSTRUCTIONS.md)**: Como publicar
- **[PROJECT_SUMMARY_PT.md](PROJECT_SUMMARY_PT.md)**: Resumo completo

## 🛠️ Tecnologias

- **Python 3.12+**
- **MetaTrader5**: Integração com plataforma de trading
- **pandas & NumPy**: Manipulação de dados
- **matplotlib**: Visualização
- **scikit-learn**: Machine Learning
- **Azure ML**: Treinamento na nuvem
- **Jupyter**: Desenvolvimento interativo

## ⚖️ Disclaimer Legal

**ESTE SOFTWARE É APENAS PARA FINS EDUCACIONAIS.**

- Trading em Forex e CFDs envolve risco significativo
- Performance passada não garante resultados futuros
- Apenas opere com dinheiro que você pode perder
- O autor não é responsável por perdas financeiras
- Sempre cumpra as regulamentações locais

Veja [LICENSE](LICENSE) para detalhes completos.

## 🚦 Scripts Auxiliares

### Verificar Segurança

Antes de fazer push para GitHub:

```bash
verify_security.bat
```

Este script verifica:
- Se .env existe localmente
- Se .env está no .gitignore
- Se há senhas hardcoded
- Se há números de conta expostos

### Inicializar Git

Para preparar o repositório:

```bash
initialize_git.bat
```

Este script:
- Inicializa Git
- Faz commits organizados
- Mostra próximos passos

## 📝 Para Publicar no GitHub

1. **Execute o verificador de segurança**:
   ```bash
   verify_security.bat
   ```

2. **Inicialize o repositório**:
   ```bash
   initialize_git.bat
   ```

3. **Crie repositório no GitHub.com**:
   - Nome: `forex-trading-bot`
   - Descrição: "Automated trading system for Forex and Gold"
   - Público

4. **Conecte e faça push**:
   ```bash
   git remote add origin https://github.com/SEU-USERNAME/forex-trading-bot.git
   git branch -M main
   git push -u origin main
   ```

5. **Configure o repositório**:
   - Adicione topics: `python`, `trading-bot`, `forex`, `metatrader5`, `fintech`
   - Ative Secret Scanning

## 💼 Para seu Portfólio

### LinkedIn

```
Acabei de publicar meu projeto de trading algorítmico! 🚀

Sistema automatizado para Forex e Ouro com:
✅ Análise em tempo real via MT5 API
✅ Execução automática com gestão de risco
✅ Integração Azure ML
✅ Código production-ready

Confira: [seu-github-link]

#Python #Trading #Fintech #MachineLearning
```

### CV

```
SISTEMA DE TRADING AUTOMATIZADO | Python, MT5, Azure ML | 2024-2025
• Desenvolveu plataforma de trading algorítmico para múltiplas moedas
• Integrou API MetaTrader 5 para dados em tempo real e execução automática
• Implementou detecção de zonas de suporte/resistência com gestão de risco
• Alcançou código production-ready com documentação abrangente
```

## 🎓 O Que Este Projeto Demonstra

### Habilidades Técnicas
- Python avançado (3.12+, type hints, async)
- Integração de APIs (MetaTrader 5)
- Análise de dados (pandas, NumPy)
- Machine Learning (scikit-learn)
- Cloud Computing (Azure ML)
- DevOps (Git, environments, CI/CD ready)

### Engenharia de Software
- Clean Code e boas práticas
- Documentação profissional
- Segurança (variáveis de ambiente)
- Tratamento de erros robusto
- Código production-ready

### Domain Knowledge
- Mercados financeiros
- Análise técnica
- Gestão de risco
- Trading algorítmico

## 🤝 Suporte

Precisa de ajuda?

1. Consulte a documentação apropriada
2. Revise USAGE.md para problemas comuns
3. Verifique SECURITY.md para questões de credenciais
4. Abra uma issue no GitHub

## 👨‍💻 Autor

**Bruno** - Entusiasta de Trading Algorítmico & Desenvolvedor Python

Desenvolvido como projeto de portfólio para demonstrar expertise em:
- Desenvolvimento de software
- Tecnologia financeira
- Ciência de dados
- Integração de sistemas

## 📞 Contato

Para oportunidades profissionais ou colaboração:
- GitHub: [Seu GitHub]
- LinkedIn: [Seu LinkedIn]
- Email: [Seu Email]

---

**⭐ Se achou interessante, considere dar uma estrela no GitHub!**

*Projeto desenvolvido com foco em qualidade, segurança e práticas profissionais para demonstrar habilidades em fintech e desenvolvimento de software.*

