<script>
	// Producto a añadir / modificar
	let product = {
		id: "",
		nombre: "",
		descripcion: "",
		categoria: "",
		imagenURL: "",
	};

	// Listado de productos
	let productos = [
		{
			id: 1,
			nombre: "Ryzen 5800X",
			categoria: "CPU",
			
		},
		{
			id: 2,
			nombre: "Radeon 6900XT",
			categoria: "GPU",
			imagenURL:
				"https://asset.msi.com/resize/image/global/product/product_160733074395dc40bc076168ef2688dc9fa5070c49.png62405b38c58fe0f07fcef2367d8a9ba1/1024.png",
		},
		{
			id: 3,
			nombre: "Corsair Crystal Series 570x",
			categoria: "ATX",
			imagenURL:
				"https://www.corsair.com/medias/sys_master/images/images/h64/h24/8896001343518/-CC-9011110-WW-Gallery-570X-RGB-WHT-01.png",
		},
	];

	let editar = false;

	const vaciarFormulario = () => {
		product = {
			id: "",
			nombre: "",
			descripcion: "",
			categoria: "",
			imagenURL: "",
		};
		editar = false;
	};

	const añadirElemento = () => {
		product.id = productos.length + 1;
		productos = productos.concat(Object.assign({}, product));
		vaciarFormulario();
	};

	const guardarElemento = () => {
		const pi = productos.findIndex(p => p.id === product.id);
		productos[pi] = Object.assign({}, product);
		vaciarFormulario();
	};

	const editarElemento = (p) => {
		product = Object.assign({}, p);
		editar = true;
	};

	const eliminarElemento = (id) => {
		productos = productos.filter(p => {
			if(p.id!==id) return p;
		})
	};

	// Handler principal
	const onSubmitHandler = (e) => {
		if (editar) {
			// Guardamos
			console.log("Guardando...");
			guardarElemento();
		} else {
			añadirElemento();
		}
	};
</script>

<main>
	<!-- Container APP-->
	<div class="container mx-auto">
		<!-- Grid de 2 Columnas con una separación 4 (1rem) -->
		<div class="grid grid-cols-2 gap-4">
			<!-- Listado de Elementos-->
			<div class="bg-gray-100 grid grid-cols-1 gap-6">
				{#each productos as p, i}
					<!-- Elemento -->
					<div
						class="bg-white rounded-lg sahdow-lg overflow-hidden border m-1 flex flex-col md:flex-row"
					>
						<div class="w-full mx-2 h-80 bg-gray-100 rounded-md p-2 my-2">
							{#if p.imagenURL}
								<img
									class="object-center object-contain w-full h-full"
									src={p.imagenURL}
									alt="thumbnail"
								/>
							{:else}
								<img
									class="object-center object-contain w-full h-full"
									src="images/no-product.jpg"
									alt="thumbnail"
								/>
							{/if}
						</div>
						<div
							class="w-full md:w-3/5 text-left p-6 md:p-4 space-y-2"
						>
							<p class="text-xl text-gray-700 font-bold">
								{p.nombre}
							</p>
							<p class="text-base text-gray-400 font-normal">
								{p.categoria}
							</p>
							<p
								class="text-base leading-relaxed text-gray-500 font-normal"
							>
								{p.descripcion || "Sin descripción"}
							</p>
							<div class="flex justify-start space-x-2">
								<button
									class="ml-5 bg-blue-200 py-1 px-2 border border-blue-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-blue-500 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-blue-300 focus:text-gray-700 focus:bg-blue-200"
									on:click={editarElemento(p)}
									><svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"
										/>
									</svg>
								</button>
								<button
									class="ml-5 bg-red-200 py-1 px-2 border border-red-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-red-500 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-red-300 focus:text-gray-700 focus:bg-red-200"
									on:click={eliminarElemento(p.id)}
									><svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
										/>
									</svg>
								</button>
							</div>
						</div>
					</div>
					<!-- Fin elemento -->
				{/each}
			</div>
			<!-- FIN Listado de Elementos-->
			<!-- Caja principal formulario-->
			<div class="p-4 shadow-md rounded-md text-left">
				<!-- on:evento cuando se envie el formulario-->
				<form on:submit|preventDefault={onSubmitHandler}>
					<!-- bind:value=variable cada cambio del input se sincorniza con la variable previamente declarada en el código -->
					<label for="nombre">Nombre del producto</label>
					<input
						bind:value={product.nombre}
						id="nombre"
						type="text"
						placeholder="Nombre del producto"
					/>
					<label for="descripcion">Descripción</label>
					<textarea
						bind:value={product.descripcion}
						id="descripcion"
						rows="3"
						placeholder="Descripción del producto"
					/>
					<label for="imagen-url">Imagen del producto</label>
					<input
						bind:value={product.imagenURL}
						type="url"
						id="imagen-url"
						placeholder="https://amazon.com/..."
					/>
					<label for="categoria">Categoría</label>
					<select bind:value={product.categoria} id="categoria">
						<option value="GPU">GPUs</option>
						<option value="CPU">CPUs</option>
						<option value="ATX">Cajas ATX y otras</option>
						<option value="Placas Base">Placas Base</option>
						<option value="RAM">RAM</option>
						<option value="RL"
							>Refrijeración Líquida (las de aire son muy feas)</option
						>
						<option value="rgb">Lucecitas</option>
					</select>
					<hr class="my-2" />
					<!-- Este boton debe de ser dual, si se añade o se modifica un elemento cambiara tanto el contenido como la función a la que va a llamar o a ejecutar...-->
					<button
						class="ml-5 bg-indigo-200 py-2 px-3 border border-indigo-500 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-indigo-500 hover:text-gray-100 focus:ring-2 focus:ring-offset-2 focus:ring-indigo-300 focus:text-gray-700 focus:bg-indigo-200"
					>
						Guardar
					</button>
				</form>
			</div>
			<!-- Fin Caja principal formulario-->
		</div>
		<!-- FIN Grid de 2 Columnas-->
	</div>
	<!-- FIN Container APP-->
</main>

<style>
</style>
