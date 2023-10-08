# Lista 3 - Transformações em Objetos 

##### Questão 1 - Desenhe uma mesma geometria 3 vezes na tela, aplicando transformações diferentes 
##### (mesmo VAO, matrizes de transformações diferentes, 3 chamadas de desenho).

*loop de execução:*
```c++
while (!glfwWindowShouldClose(window))
{
	// Checa se houveram eventos de input (key pressed, mouse moved etc.) e chama as fun��es de callback correspondentes
	glfwPollEvents();

	// Limpa o buffer de cor
	glClearColor(0.0f, 0.0f, 0.0f, 1.0f); //cor de fundo
	glClear(GL_COLOR_BUFFER_BIT);

	glLineWidth(10);
	glPointSize(20);

	// Recuperando o tamanho da janela da aplica��o
	glfwGetFramebufferSize(window, &width, &height);
	// Dimensiona a viewport
	glViewport(0, 0, width, height);
	glBindVertexArray(VAO); //Conectando ao buffer de geometria


	//Matriz de transforma��es no objeto - Matriz de modelo
	glm::mat4 model = glm::mat4(1); //matriz identidade

	float angle = (float)glfwGetTime();
	//Aplicando as transforma��es
	model = glm::translate(model, glm::vec3(400.0, 300.0, 0.0));
	model = glm::rotate(model, /*glm::radians(45.0f)*/angle, glm::vec3(0.0, 0.0, 1.0));
	model = glm::scale(model, glm::vec3(500.0, 500.0, 1.0));

	shader.setMat4("model", glm::value_ptr(model));
	shader.setVec4("inputColor", 0.0, 0.0, 1.0, 1.0);

	glViewport(0, height / 2, width / 2, height / 2);
	glDrawArrays(GL_TRIANGLES, 0, 3);
	shader.setVec4("inputColor", 1.0, 1.0, 1.0, 1.0);
	glDrawArrays(GL_POINTS, 0, 3);

	glViewport(width / 2, height / 2, width / 2, height / 2);
	shader.setVec4("inputColor", 1.0, 0.0, 0.0, 1.0);
	glDrawArrays(GL_TRIANGLES, 0, 3);
	shader.setVec4("inputColor", 1.0, 1.0, 1.0, 1.0);
	glDrawArrays(GL_POINTS, 0, 3);

	glViewport(width / 2, 0, width / 2, height / 2);
	shader.setVec4("inputColor", 0.0, 1.0, 0.0, 1.0);
	glDrawArrays(GL_TRIANGLES, 0, 3);
	shader.setVec4("inputColor", 1.0, 1.0, 1.0, 1.0);
	glDrawArrays(GL_POINTS, 0, 3);

	glBindVertexArray(0); //Desconectando o buffer de geometria

	// Troca os buffers da tela
	glfwSwapBuffers(window);
}
```

<img src = img/lista3/q1>


##### Questão 2 - Crie uma cena composta de quadrados coloridos dispostos em grid, como o exemplo abaixo:

Neste caso, basta criar um for que vai montando as imagens de acordo com o point of view.
Eu optei por fazer um grid 8x8, com cores que vão trocando com o tempo.
```c++
int cellWidth = width / 8;
int cellHeight = height / 8;

for (int i = 0; i < 8; ++i) {
	for (int j = 0; j < 8; ++j) {
		glViewport(i * cellWidth, j * cellHeight, cellWidth, cellHeight);

		// Gera cores RGB aleatórias
		float r = static_cast<float>(std::rand()) / static_cast<float>(RAND_MAX);
		float g = static_cast<float>(std::rand()) / static_cast<float>(RAND_MAX);
		float b = static_cast<float>(std::rand()) / static_cast<float>(RAND_MAX);

		shader.setVec4("inputColor", r, g, b, 1.0f);

		glDrawArrays(GL_TRIANGLES, 0, 3);
		shader.setVec4("inputColor", 1.0, 1.0, 1.0, 1.0);
		glDrawArrays(GL_POINTS, 0, 3);
	}
}
```
<img src = img/lista3/q2>
