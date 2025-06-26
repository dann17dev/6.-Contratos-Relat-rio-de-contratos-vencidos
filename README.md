## 6. Contratos: Relatório de contratos vencidos

### 🛠 Problema real
Em muitas empresas que usam o Protheus, o controle de vencimento de contratos (aluguel, prestação de serviço, manutenção, etc.) é feito manualmente ou mal gerenciado. A diretoria ou setor responsável **só percebe o vencimento quando o serviço para ou o custo aumenta**, gerando prejuízo ou perda de garantias.

### 📉 Impacto
- Contratos vencidos sem renovação ou aviso prévio
- Pagamentos indevidos ou cobranças inesperadas
- Perda de serviços, garantias ou condições contratuais
- Exposição jurídica por falta de gestão

### 💡 Solução aplicada
Desenvolvimento de um relatório automático que **lista todos os contratos vencidos ou a vencer** em um intervalo de tempo (por padrão, últimos 30 dias ou próximos 15 dias). A fonte pode ser a tabela `CN9` ou `CNU`, dependendo da configuração de contratos do cliente.

O relatório pode ser enviado por e-mail para os gestores responsáveis semanalmente ou ser disponibilizado via dashboard.

### 🧾 Código exemplo (ADVPL)
```advpl
User Function ContrVen()
    Local dHoje := dDataBase
    Local dLimite := dHoje - 30
    Local cLinha := ""
    Local cArquivo := "C:\relatorios\ContratosVencidos.csv"

    MemoWrite(cArquivo, "Contrato;Cliente;Data Início;Data Fim;Situação" + CRLF)

    dbSelectArea("CN9")
    dbGoTop()

    While !Eof()
        If CN9->CN9_DTFIM <= dHoje
            cLinha := CN9->CN9_NUMCON + ";" +;
                      CN9->CN9_CLIENT + ";" +;
                      Dtoc(CN9->CN9_DTINI) + ";" +;
                      Dtoc(CN9->CN9_DTFIM) + ";" +;
                      "Vencido" + CRLF
            MemoWrite(cArquivo, cLinha, .T.)
        EndIf
        dbSkip()
    EndDo

    MsgInfo("Relatório de contratos vencidos gerado.")
Return
```
> A consulta pode ser expandida com contratos a vencer nos próximos 15 dias para alertas preventivos.

## 🧪 Casos de Teste - Validação de Contratos

| Status do Contrato       | Dias para/From Vencimento | Ação do Sistema                | Gravidade |
|--------------------------|--------------------------|--------------------------------|-----------|
| Vencido                  | +10 dias                 | Inclusão automática no relatório | Alta (✅) |
| Pré-vencimento           | -5 dias                  | Alerta amarelo (opcional)       | Média (⚠️) |
| Vigente                  | +30 dias                 | Não reportado                  | N/A (🚫)  |

> Para adicionar regras de negócio:
> **Regra:** Contratos com vencimento entre -15 e +5 dias são considerados "em alerta"

| Código Erro | Mensagem para Usuário          |
|-------------|--------------------------------|
| CT-001      | "Contrato XYZ vencido em DD/MM" |
| CT-005      | "Alerta: contrato vencerá em 5 dias" |

🎯 Benefícios
Diretores e gestores têm visão clara dos vencimentos

Redução de riscos contratuais e jurídicos

Agilidade em renovações, negociações e cancelamentos

Geração automática de alertas com baixo custo técnico

🏷️ Tags
#Protheus #Contratos #CN9 #Gestão #Relatórios #ADVPL #Backoffice

