# AVLTree
## Alan Albino Tellez Giron Lizarraga

### Estructura de datos

```c++
#ifndef AVLTREE_H
#define AVLTREE_H
#include <iostream>
```
Este bloque de codigo es una directiva de preprocesador para evitar que el contenido del archivo de encabezado
se incluya mas ded una vez en un mismo archivo durante la compilacion.

```c++
struct Node {
    int value;
    Node* left;
    Node* right;
    int height;
};
```
Este fragmento de codigo define un Struct llamado Node
el cual tiene los siguientes campos:

```c++
int value;
```
Este campo almacena el valor numerico asociado con el nodo

```c++
Node* left;
```
Este es un puntero que apunta al hijo izquierdo del nodo actual
```c++
Node* right;
```
Este es un puntero que apunta al hijo izquierdo del nodo actual
```c++
int height
```
Este campo almacena la altura del nodo en el arbol

```c++
class AVLTree {
    public:
    void printTree() {
        printTree(root, 0, 10);
    }
private:
    Node* root;
```
Este fragmento de codigo proporcionado define una clase llamada AVLTree
```c++
class AVLTree
```
Define una clase llamada AVLTree
```c++
Metodo printTree()
```
Este metodo imprime el arbol AVL
```c++
private Node* root;
```
Este es un puntero a un objeto de tipo Node que representa la raiz del arbol
```c++
  int getNodeHeight(Node* N) {
        if (N == nullptr)
            return 0;
        return N->height;
    }
```
Esta funcion calcula altura de un nodo arbol AVL que devuelve la altura del nodo N si existe y es distinto a nulo, si es nulo
devuelve 0;
```c++
 int maxHeight(int a, int b) {
        return (a > b)? a : b;
    }
```
Esta funcion calcula la altura de los nodos y determinar el balance del arbol

```c++
    Node* newNode(int value) {
        Node* node = new Node();
        node->value = value;
        node->left = nullptr;
        node->right = nullptr;
        node->height = 1;
        return(node);
    }
```
Esta funcion se usa para inicializar un nuevo nodo antes de insertarlo en el arbol
```c++
  Node* rightRotate(Node* y) {
        Node* x = y->left;
        Node* T2 = x->right;

        x->right = y;
        y->left = T2;

        y->height = maxHeight(getNodeHeight(y->left), getNodeHeight(y->right)) + 1;
        x->height = maxHeight(getNodeHeight(x->left), getNodeHeight(x->right)) + 1;

        return x;
    }
```
Esta funcion realiza una rotacion hacia la derecha alrededor del Nodo y, esto se usa para mantener el balance del arbol despues
de la insercion o eliminacion de nodos.
```c++
  Node* leftRotate(Node* x) {
        Node* y = x->right;
        Node* T2 = y->left;

        y->left = x;
        x->right = T2;

        x->height = maxHeight(getNodeHeight(x->left), getNodeHeight(x->right)) + 1;
        y->height = maxHeight(getNodeHeight(y->left), getNodeHeight(y->right)) + 1;

        return y;
    }
```
Esta funcion realiza una rotacion hacia la izquierda alrededor del nodo x, esta funcion se usa para mantener el balance del arbol 
despues de la insercion o eliminacion de nodos.
```c++
 int getBalance(Node* N) {
        if (N == nullptr)
            return 0;
        return getNodeHeight(N->left) - getNodeHeight(N->right);
    }
```
Esta funcion sirve para calcular el factor de balance de un nodo dado. El factor de balance de un nodo en un arbol AVL es la diferencia de alturas
del subarbol izquierdo y derecho del nodo. Esta funcion se utiliza para determinar si se requieren ajustes (rotaciones) para mantener el balance del arbol
```c++
 Node* insertNode(Node* node, int value) {
        if (node == nullptr)
            return(newNode(value));

        if (value < node->value)
            node->left = insertNode(node->left, value);
        else if (value > node->value)
            node->right = insertNode(node->right, value);
        else
            return node;

        node->height = 1 + maxHeight(getNodeHeight(node->left), getNodeHeight(node->right));

        int balance = getBalance(node);

        if (balance > 1 && value < node->left->value)
            return rightRotate(node);

        if (balance < -1 && value > node->right->value)
            return leftRotate(node);

        if (balance > 1 && value > node->left->value) {
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }

        if (balance < -1 && value < node->right->value) {
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }

        return node;
    }
```
La funcion insertNode se utiliza para insertar un nuevo nodo con un valor dado en el arbolAVL
la funcion recibe un puntero al nodo node que representa la raiz del subarbol en el que se va a insertar el nuevo nodo y un
valor entero value que se va a insertar en el arbol.
primero se comprueba si el nodo actual es nulo, si es nulo significa que se ha alcanzado una posicion vacia en el arbol
donde se puede insertar el nuevo nodo y un valor value.  
Si el nodo actual no es nulose compara el valor a insertar con el valor del nodo actual.
Si el valor a insertar es menor que el valor del nodo actual se insertara en el lado izquierdo del nodo raiz. Por otro lado si el valor a insertar
es mayor entonces se insertara en el lado derecho del nodo actual. Si el valor a insertar
es igual al valor deol nodo actual se devuelve el nodo actual sin realizar ninguna insercion porque el valor ya existe en el arbol-
Despues de insertar el nodo se actualiza la altura del nodo actual.
Despues de actualizar la altura del nodo se calcula el factor de balance del nodo utilizando la funcion getBalance. Si el factor
de balance indica un desequilibrio en el arbol, se realizan rotaciones para restaurar el balance con las funciones rightRotate y leftRotate-
Finalmente se devuelve el nodo actualizado.

```c++
   Node* minValueNode(Node* node) {
        Node* current = node;

        while (current->left != nullptr)
            current = current->left;

        return current;
    }
```
Esta funcion se utiliza para encontrar el nodo con el valor minimo, esto se utiliza luego para reemplazar el nodo a eliminar debido a que al eliminar un nodo con 2 hijos se encuentra el sucesor inmmediato del nodo a eliminar que es el nodo con el valor minimo
en el subarbol derecho del nodo a eliminar
```c++
    Node* deleteNode(Node* root, int value) {
        if (root == nullptr)
            return root;

        if ( value < root->value )
            root->left = deleteNode(root->left, value);
        else if( value > root->value )
            root->right = deleteNode(root->right, value);
        else {
            if( (root->left == nullptr) || (root->right == nullptr) ) {
                Node *temp = root->left ? root->left : root->right;

                if(temp == nullptr) {
                    temp = root;
                    root = nullptr;
                }
                else
                    *root = *temp;

                delete temp;
            }
            else {
                Node* temp = minValueNode(root->right);

                root->value = temp->value;

                root->right = deleteNode(root->right, temp->value);
            }
        }
```
Esta funcion se utiliza para eliminar un nodo con un valor dado y garantiza que despues de la eliminacion el arbol permanezva balanceado.
```c++
   if (root == nullptr)
            return root;

        root->height = 1 + maxHeight(getNodeHeight(root->left), getNodeHeight(root->right));

        int balance = getBalance(root);

        if (balance > 1 && getBalance(root->left) >= 0)
            return rightRotate(root);

        if (balance > 1 && getBalance(root->left) < 0) {
            root->left = leftRotate(root->left);
            return rightRotate(root);
        }

        if (balance < -1 && getBalance(root->right) <= 0)
            return leftRotate(root);

        if (balance < -1 && getBalance(root->right) > 0) {
            root->right = rightRotate(root->right);
            return leftRotate(root);
        }

        return root;
    }
```
Despues de eliminar un nodo en la funcion deleteNode es necesario realizar ajustes para mantener el balance del arbol.
Primero se comprueba si el nodo root es nulo. Si es nulo significa que el arbol esta vacio por lo que no se realizan mas ajustes y 
devuelve el nodo root. 
Si no es nulo se actualiza la altura del nodo root. La altura se calcula como 1 mas el maximo entre la altura del subarbol izquierdo 
y la altura del subarbol derecho del nodo.
Luego se calcula el factor de balance del nodo root usando la funcion getBalance.
Antes de empezar restaurar el balance se verifican cuatro casos para determinar si se necesita realizar rotaciones para 
ya empezar a restaurar el balance del arbol.  
1 Si el factor de balance es mayor que 1 y el factor de balance del subarbol izquierdo es mayor o igual a 0, se realizara una rotacion hacia la derecha  
2 Si el factor de balance es mayor que 1 y el factor de balance del subarbol izquierdo es menor que 0, se realiza una rotacion izquierda a derecha  
3 Si el factor de balance es menor que -1 y el factor de balance del subarbol derecho es menor o igual a 0, se realiza una rotacion hacia la izquierda  
4 Si el factor de balance es menor que -1 y el factor de balance derecho es mayor que 0, se realizara una rotacion derecha a izquierda  
Despues de realizar los ajustes necesarios se devuelve el nodo root.
```c++
   void deleteTree(Node* node) {
        if (node == nullptr)
            return;

        deleteTree(node->left);
        deleteTree(node->right);

        delete node;
    }
```
La funcion deleteTree se usa para eliminar todos los nodos del arbolAVL, liberando la memoria asignada dinamicamente para cada nodo.
```c++
public:
    AVLTree() : root(nullptr) {}
```
Esta es la definicion del constructor predeterminado de la clase AVLTree.
```c++
  ~AVLTree() {
        deleteTree(root);
    }
```
Este es el destructro de la clase AVLTree
```c++

    void insert(int value) {
        root = insertNode(root, value);
    }
```
Esta es la implementacion del metodo insert de la clase AVLTree
```c++
  void remove(int value) {
        root = deleteNode(root, value);
    }
```
Este es el metodo remove de la clase AVLTree que permite eliminar un nodo del arbol AVL y garantiza que el arbol permanezca balanceado segun las
reglas de un arbol AVL.
```c++
  void inorderTraversal(Node* node) {
        if (node == nullptr)
            return;

        inorderTraversal(node->left);
        std::cout << node->value << " ";
        inorderTraversal(node->right);
    }
```
Este es el metodo InorderTraversal de la clase AVLTree que funciona para imprimir los valores de todos los nodos en el arbol AVL en orden ascendente.
```c++
  void preorderTraversal(Node* node) {
        if (node == nullptr)
            return;

        std::cout << node->value << " ";
        preorderTraversal(node->left);
        preorderTraversal(node->right);
    }
```
Este es el metodo preorderTraversal que imprime los valores de todos los nodos en el arbol AVL en preorden. (de arriba hacia abajo)

```c++
  void postorderTraversal(Node* node) {
        if (node == nullptr)
            return;

        postorderTraversal(node->left);
        postorderTraversal(node->right);
        std::cout << node->value << " ";
    }
```
Este es el metodo postorderTraversal que imprime los valores de todos los nodos en el arbol ABL en postorden (de abajo hacia arriba)
```c++
 void printTree(Node* root, int space = 0, int COUNT = 10) {
        if (root == nullptr) {
            return;
        }

        space += COUNT;
        printTree(root->right, space);

        std::cout << std::endl;

        for (int i = COUNT; i < space; i++) {
            std::cout << " ";
        }

        std::cout << root->value << "\n";

        printTree(root->left, space);
    }
    };
```
Este metodo proporciona una representacion grafica del arbol AVL en la consola con los nodos organizados jerarquicamente y separados por espacios para mostrar su
posicion relativa en el arbol.
```c++
#endif // AVLTREE_H
```
esta es una directiva de preprocesador que finaliza la proteccion de inclusion condicional iniciada por ifndef AVLTREE_H