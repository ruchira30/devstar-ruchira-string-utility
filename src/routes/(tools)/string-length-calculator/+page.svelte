<script lang="ts">
    import { Button } from 'flowbite-svelte';
    import jsPDF from 'jspdf';
    import Intro from '$lib/Intro.svelte';

    export let data;
    let inputText = '';
    let strlength = '';
  
    
	function calculateLength() {
        strlength=inputText.length.toString();
	}

    function copyText() {
        if (inputText.length > 0) {
            var textarea = document.createElement("textarea");
            textarea.value = `InputText:  ${inputText}\nLength: ${strlength}`;
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand("copy");
            document.body.removeChild(textarea);
        }
    }
    function downloadText() {
    if (strlength.length > 0) {
        const combinedText=`InputText: ${inputText}\n\nLength: ${strlength}`;
        const textBlob= new Blob([combinedText], { type: 'text/plain' });
    

        const anchor = document.createElement('a');
        anchor.href = URL.createObjectURL(textBlob);
       

        anchor.download = 'devstar_output.txt';
        anchor.click();

        URL.revokeObjectURL(anchor.href);
    }
}

	function downloadPDF() {
        if (strlength.length > 0) {
            const pdf = new jsPDF();
            pdf.text("InputText:", 10, 10);
            pdf.text(inputText, 40, 10); 
            pdf.text("Length:", 10, 20);
            pdf.text(strlength, 30, 20); 
            pdf.save("devstar_output.pdf"); 
        }
    }
</script>

<Intro heading={data.meta.title} description={data.meta.description} />

<section class="bg-white dark:bg-gray-900">
    <div class="py-8 px-4 mx-auto max-w-screen-xl lg:px-12">
        <div class="card p-8 relative items-center mx-auto max-w-screen-xl overflow-hidden rounded-lg">

            <div class="gap-4 items-center mx-auto max-w-screen-xl lg:grid lg:grid-cols-2 overflow-hidden">
                <div class="rounded-lg overflow-hidden bg-gray-50 border border-gray-300">
                    <textarea placeholder="Enter Text" rows="8" class="resize-none block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    bind:value={inputText} on:input={calculateLength}/>
                </div>
                <div class="rounded-lg overflow-hidden bg-gray-50 border border-gray-300">
                    <textarea readonly placeholder="Result" rows="8" class="resize-none block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" 
                    bind:value={strlength}/>
                </div>
            </div>

			<div class="items-center mx-auto max-w-screen-xl lg:grid lg:grid-cols-1 overflow-hidden">
				<div class="mt-8 gap-4 items-center mx-auto max-w-screen-xl lg:grid lg:grid-cols-3 overflow-hidden">
					<Button color="blue" on:click={copyText}>Copy</Button>
					<Button color="blue" on:click={downloadText}>Download as txt</Button>
					<Button color="blue" on:click={downloadPDF}>Download as pdf</Button>
				</div>	
			</div>

        </div>
    </div>
</section>

<style>  

    .card {
        box-shadow: rgba(0, 0, 0, 0.1) 0 0 0 2px;
    }

    :is(.dark .card) {
        box-shadow: rgba(255, 255, 255, 0.5) 0 0 0 2px;
    }

</style>