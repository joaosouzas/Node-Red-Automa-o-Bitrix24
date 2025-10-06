# Node-Red-Automa-o-Bitrix24
Automação com Bitrix para empresas, aonde lê os cards listados nas pipelines e cada atualização envia a informação para o Node-Red e encaminha uma mensagem no Teams. 

Segunda automação com um gatilho do Node-Red, criei uma mensagem para pegar uma etapa especifica da pipeline aonde irá enviar de seg a sex as 16hrs Horário de Brasíla os cards atrasados, baseado no campo de hora do card.

Cada campo foi encontrado com um nó de debug testanod manualmente até vir os o nome dos campos da pipeline, lembrando que cada campo C6: UC varia de empresa para empresa, então tem que modificar e testar o fluxo.

Cada campo pode ser alterável e dinâmico, o caso é para cada finalidade, coloquei um href no payload.result para pegar os link que utilizamos na empresa, verifique a necessidade.

Para utilizar o flow abaixo, pode importar ele no node-red ou utilizar o Visual Studio Code, utilizei o Node-red para facilitar a automação, mas todo o códio é em JavaScript, aconselho usar o VSCode para alterações.

No bitrix os webhooks de entrada devem ser utilizados com a permissão de CRM apenas, aonde lista todos do negócio e deixa mais abrangente.
