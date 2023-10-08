# graphicProcessing
Repository used to store my codes and projects related to graphic processing class at university

To create Circles:
```c++
int setupGeometry()
{
	const int points = 360; // número de pontos
	GLfloat vertices[3 * (points + 2)]; // array para armazenar os vértices do círculo
	float centerX = 0.0f; // coordenada x do centro
	float centerY = 0.0f; // coordenada y do centro
	float radius = 1.0f; // raio do círculo

	// ângulo entre pontos
	float angle = 360.0f / (points) ;

	// ponto central
	vertices[0] = centerX;
	vertices[1] = centerY;
	vertices[2] = 0; // coordenada z

	for (int i = 0; i <= points; ++i) {
		// ângulo atual
		float theta = angle * i;

		// calcular xy
		float x = centerX + radius * scale * cos(theta * M_PI / 180.0f); // coordenada x
		float y = centerY + radius * scale * sin(theta * M_PI / 180.0f); // coordenada y

		vertices[(i + 1) * 3] = x;
		vertices[(i + 1) * 3 + 1] = y;
		vertices[(i + 1) * 3 + 2] = 0; // coordenada z
	}

	GLuint VBO, VAO;
	glGenBuffers(1, &VBO);
	glBindBuffer(GL_ARRAY_BUFFER, VBO);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

	glGenVertexArrays(1, &VAO);
	glBindVertexArray(VAO);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(GLfloat), (GLvoid*)0);
	glEnableVertexAttribArray(0);

	glBindBuffer(GL_ARRAY_BUFFER, 0);
	glBindVertexArray(0);

	return VAO;
}

```
<img src = img/circulo>

To create spirals, we just need to switch so that our radius increases as a function of theta:
```C++

//glDrawArrays(GL_LINE_STRIP, 0, num_segments + 1); //Line strip to remove the vertice (1,n)

int setupGeometry()
{
	const int num_segments = 100; // número de segmentos usados para desenhar o círculo
	GLfloat vertices[3 * (num_segments + 1)]; // array para armazenar os vértices do círculo

	for (int i = 0; i <= num_segments; ++i) {
		float theta = 4.0f * 3.1415926f * float(i) / float(num_segments); // ângulo atual
		vertices[i * 3] = 0.1 * theta * cosf(theta); // coordenada x
		vertices[i * 3 + 1] = 0.1 * theta * sinf(theta); // coordenada y
		vertices[i * 3 + 2] = 0; // coordenada z
	}

	GLuint VBO, VAO;
	glGenBuffers(1, &VBO);
	glBindBuffer(GL_ARRAY_BUFFER, VBO);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

	glGenVertexArrays(1, &VAO);
	glBindVertexArray(VAO);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(GLfloat), (GLvoid*)0);
	glEnableVertexAttribArray(0);

	glBindBuffer(GL_ARRAY_BUFFER, 0);
	glBindVertexArray(0);

	return VAO;
}
```
<img src = img/espiral>
Pac-Man

```c++
glDrawArrays(GL_TRIANGLE_FAN, 0, 300);
int setupGeometry()
{
	const int points = 360; // número de pontos
	GLfloat vertices[3 * (points + 2)]; // array para armazenar os vértices do círculo
	float centerX = 0.0f; // coordenada x do centro
	float centerY = 0.0f; // coordenada y do centro
	float radius = 1.0f; // raio do círculo

	// ângulo entre pontos
	float angle = 360.0f / (points);

	// ponto central
	vertices[0] = centerX;
	vertices[1] = centerY;
	vertices[2] = 0; // coordenada z

	for (int i = 0; i < points - 1; ++i) {
		// ângulo atual
		float theta = angle * i + (360.0f / 15.0f);

		// calcular xy
		float x = centerX + radius * scale * cos(theta * M_PI / 180.0f); // coordenada x
		float y = centerY + radius * scale * sin(theta * M_PI / 180.0f); // coordenada y

		vertices[(i + 1) * 3] = x;
		vertices[(i + 1) * 3 + 1] = y;
		vertices[(i + 1) * 3 + 2] = 0; // coordenada z
	}

	GLuint VBO, VAO;
	glGenBuffers(1, &VBO);
	glBindBuffer(GL_ARRAY_BUFFER, VBO);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

	glGenVertexArrays(1, &VAO);
	glBindVertexArray(VAO);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(GLfloat), (GLvoid*)0);
	glEnableVertexAttribArray(0);

	glBindBuffer(GL_ARRAY_BUFFER, 0);
	glBindVertexArray(0);

	return VAO;
}
```

Pizza
```c++
//glDrawArrays(GL_TRIANGLE_FAN, 0, 20);
int setupGeometry()
{
	const int points = 300; // número de pontos
	GLfloat vertices[3 * (points + 2)]; // array para armazenar os vértices do círculo
	float centerX = 0.0f; // coordenada x do centro
	float centerY = 0.0f; // coordenada y do centro
	float radius = 1.0f; // raio do círculo

	// ângulo entre pontos
	float angle = 360.0f / points;

	// ponto central
	vertices[0] = centerX;
	vertices[1] = centerY;
	vertices[2] = 0; // coordenada z

	for (int i = 0; i < points; ++i) {
		// ângulo atual
		float theta = angle * i;

		if (i >= points / 4 && i <= points - points / 4) {
			theta += (360 / 15);
		}

		// calcular xy
		float x = centerX + radius * scale * cos(theta * M_PI / 180.0f); // coordenada x
		float y = centerY + radius * scale * sin(theta * M_PI / 180.0f); // coordenada y

		vertices[(i + 1) * 3] = x;
		vertices[(i + 1) * 3 + 1] = y;
		vertices[(i + 1) * 3 + 2] = 0; // coordenada z
	}

	GLuint VBO, VAO;
	glGenBuffers(1, &VBO);
	glBindBuffer(GL_ARRAY_BUFFER, VBO);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

	glGenVertexArrays(1, &VAO);
	glBindVertexArray(VAO);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(GLfloat), (GLvoid*)0);
	glEnableVertexAttribArray(0);

	glBindBuffer(GL_ARRAY_BUFFER, 0);
	glBindVertexArray(0);

	return VAO;
}

```

Estrela
```c++
int setupGeometry()
{
	const int points = 10; // número de pontos
	GLfloat vertices[3 * (points + 2)]; // array para armazenar os vértices do círculo
	float centerX = 0.0f; // coordenada x do centro
	float centerY = 0.0f; // coordenada y do centro
	float radius = 1.0f; // raio do círculo

	// ângulo entre pontos
	float angle = 360.0f / (points) + 101;

	// ponto central
	vertices[0] = centerX;
	vertices[1] = centerY;
	vertices[2] = 0; // coordenada z

	for (int i = 0; i <= points; ++i) {
		// ângulo atual
		float theta = angle * i;

		// calcular xy
		float x = centerX + radius * scale * cos(theta * M_PI / 180.0f); // coordenada x
		float y = centerY + radius * scale * sin(theta * M_PI / 180.0f); // coordenada y

		vertices[(i + 1) * 3] = x;
		vertices[(i + 1) * 3 + 1] = y;
		vertices[(i + 1) * 3 + 2] = 0; // coordenada z
	}

	GLuint VBO, VAO;
	glGenBuffers(1, &VBO);
	glBindBuffer(GL_ARRAY_BUFFER, VBO);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

	glGenVertexArrays(1, &VAO);
	glBindVertexArray(VAO);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(GLfloat), (GLvoid*)0);
	glEnableVertexAttribArray(0);

	glBindBuffer(GL_ARRAY_BUFFER, 0);
	glBindVertexArray(0);

	return VAO;
}
```
