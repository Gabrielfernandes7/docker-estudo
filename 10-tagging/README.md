# Tagging

## Boas práticas de Tagging em imagem Docker

Tagging (etiquetar) é dar nomes claros e úteis às suas imagens Docker. Um bom tagging evita confusão, facilita rollbacks, melhora rastreabilidade e previne problemas em produção.

Problemas comuns com tagging ruim:
- Usar só `:latest` → não sabe qual versão está rodando
- Tags mutáveis em produção → deploy quebra sem aviso
- Sem rastreio → difícil saber de qual commit/código veio a imagem
- Registry lotado de tags antigas → difícil limpar e gerenciar

### Princípios principais

1. **Evite o tag `:latest` em produção**  
   O `:latest` é mutável (pode mudar para uma nova build sem você perceber).  
   Sempre use tags específicas e imutáveis (que não mudam depois de criadas).  
   Use `:latest` só em dev ou testes locais (para conveniência).

   Ruim (produção):
   ```yaml
   image: minha-app:latest   # Qual versão é essa? Pode mudar sozinha!

