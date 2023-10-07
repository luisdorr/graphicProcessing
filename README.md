# graphicProcessing
Repository used to store my codes and projects related to graphic processing class at university
Para criação de circulos:
```c++
int setupGeometry()
{
    const int num_segments = 100; // número de segmentos usados para desenhar o círculo
    GLfloat vertices[3*(num_segments+1)]; // array para armazenar os vértices do círculo

    for (int i = 0; i <= num_segments; ++i) {
        float theta = 2.0f * 3.1415926f * float(i) / float(num_segments); // ângulo atual
        vertices[i*3] = cosf(theta); // coordenada x
        vertices[i*3+1] = sinf(theta); // coordenada y
        vertices[i*3+2] = 0; // coordenada z
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
