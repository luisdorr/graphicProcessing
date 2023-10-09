# Lista 3 - Transformações em Objetos 

#### Questão 1 - Desenhe uma mesma geometria 3 vezes na tela, aplicando transformações diferentes 
#### (mesmo VAO, matrizes de transformações diferentes, 3 chamadas de desenho).

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

<img src = img/lista3/q1.png>


#### Questão 2 - Crie uma cena composta de quadrados coloridos dispostos em grid, como o exemplo abaixo:

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
<img src = img/lista3/epilepsia.gif>



#### Questão 3 -

##### Movimentação 
Source.cpp
```c++
void key_callback(GLFWwindow* window, int key, int scancode, int action, int mode)
{
	if (key == GLFW_KEY_ESCAPE && action == GLFW_PRESS)
		glfwSetWindowShouldClose(window, GL_TRUE);

	if (key == GLFW_KEY_A || key == GLFW_KEY_LEFT)
	{
		faca.setState(MOVING_LEFT);
		faca.moveLeft();
	}
	else if (key == GLFW_KEY_D || key == GLFW_KEY_RIGHT)
	{
		faca.setState(MOVING_RIGHT);
		faca.moveRight();
	}
	else if (key == GLFW_KEY_W || key == GLFW_KEY_UP)
	{
		faca.setState(MOVING_UP);
		faca.moveUp();
	}
	else if (key == GLFW_KEY_S || key == GLFW_KEY_DOWN)
	{
		faca.setState(MOVING_DOWN);
		faca.moveDown();
	}
	if (action == GLFW_RELEASE) //soltou a tecla
	{
		faca.setState(IDLE);
	}
}
```
Sprite.cpp
```c++
void Sprite::moveLeft()
{
	position.x -= vel;
}

void Sprite::moveRight()
{
	position.x += vel;
}

void Sprite::moveUp()
{
	position.y += vel;
}

void Sprite::moveDown()
{
	position.y -= vel;
}
```
###### Figura
```c++
this->nAnimations = nAnimations;
this->nFrames = nFrames;
ds = 1.0 / (float)nFrames;
dt = 1.0 / (float)nAnimations;
iFrame = 0;
iAnimation = 0;

GLfloat vertices[] = {
	//Primeiro Triângulo
	// Lâmina da faca
	 0.4f, 0.0f, 0.0f, 0.0, 0.0, //v0
	 1.5f, 0.0f, 0.0f, ds, dt, //v1
	 0.4f, -0.3f, 0.0f, 0.0, dt, //v2
	// Cabo
	 0.4f,  -0.15f, 0.0f, 0.0, 0.0,
	-0.2f,  -0.15f, 0.0f, ds, dt,
	-0.2f,  -0.00f, 0.0f, 0.0, dt,
	-0.2f,  -0.00f, 0.0f, 0.0, 0.0,
	 0.4f,  -0.00f, 0.0f, ds, dt,
	 0.4f,  -0.15f, 0.0f, 0.0, dt,
};
```
<img src = img/lista3/faca.gif>

