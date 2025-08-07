<script>
    import { onMount } from "svelte";
  
    let questoes = [];      // Todas as questões carregadas do JSON
    let pesquisa = "";      // Texto digitado na barra de pesquisa
    let resultados = [];    // Questões filtradas pela busca
  
    // Carregar as questões ao montar o componente
    onMount(async () => {
      const res = await fetch("/todas_questoes.json");
      questoes = await res.json();
    });
  
    // Função para buscar as questões
    function buscar() {
      if (pesquisa.trim() === "") {
        resultados = [];
        return;
      }
  
      const termo = pesquisa.toLowerCase();
  
      resultados = questoes.filter(q =>
        q.text.toLowerCase().includes(termo) ||
        q.correct_answer.toLowerCase().includes(termo)
      );
    }
</script>
  

<div class="flex flex-col items-center min-h-screen p-6 bg-gray-100 font-[Poppins]">
    <h1 class="text-3xl font-black text-gray-800 mb-4">Buscar Questões</h1>
    <p class="text-gray-600 mb-6">Total de questões no banco: <strong>{questoes.length}</strong></p>
    
    <!-- Barra de pesquisa -->
    <input
        type="text"
        bind:value={pesquisa}
        on:input={buscar}
        placeholder="Digite sua pesquisa..."
        class="w-full max-w-lg px-4 py-2 text-lg border border-gray-300 rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none"
    />
    
    <!-- Exibir os resultados da busca -->
    {#if resultados.length > 0}
        <ul class="mt-6 w-full max-w-lg max-h-96 bg-white p-4 rounded-lg shadow-md overflow-scroll">
        {#each resultados as questao}
            <li class="border-b border-gray-200 py-3">
            <strong class="text-blue-600">{questao.number}.</strong> {questao.text}  
            <br />
            ✅ <span class="text-green-600 font-semibold">{questao.correct_answer}</span>
            </li>
        {/each}
        </ul>
    {:else if pesquisa.trim() !== ""}
        <p class="mt-4 text-gray-500">Nenhuma questão encontrada.</p>
    {/if}
</div>


  