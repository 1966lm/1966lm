# Resolver Sopa de Letras con Google Colab
import json

def find_word(letter_soup, word):
    """
    Verifica si una palabra está en la sopa de letras en alguna dirección válida.
    Entrada: letter_soup (list[list[str]]), word (str)
    Salida: bool
    """
    n, m = len(letter_soup), len(letter_soup[0])
    word_len = len(word)
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1), (-1, -1), (1, 1), (-1, 1), (1, -1)]

    def valid(x, y):
        return 0 <= x < n and 0 <= y < m

    def check_direction(x, y, dx, dy):
        for k in range(word_len):
            nx, ny = x + k * dx, y + k * dy
            if not valid(nx, ny) or letter_soup[nx][ny] != word[k].upper():  # Compara en mayúsculas
                return False
        return True

    for i in range(n):
        for j in range(m):
            if letter_soup[i][j] == word[0].upper():  # Compara en mayúsculas
                for dx, dy in directions:
                    if check_direction(i, j, dx, dy):
                        return True
    return False

def find_words(letter_soup, words):
    """
    Verifica cuáles palabras de una lista están en la sopa de letras.
    Entrada: letter_soup (list[list[str]]), words (list[str])
    Salida: dict[str, bool]
    """
    return {word: find_word(letter_soup, word) for word in words}

def save_results_to_json(results, output_path):
    """
    Guarda los resultados en un archivo JSON.
    Entrada: results (dict[str, bool]), output_path (str)
    Salida: None
    """
    with open(output_path, 'w') as json_file:
        json.dump(results, json_file, indent=4)

def main():
    """
    Función principal que ejecuta el flujo completo del programa.
    """
    # Sopa de letras incluida directamente en el código
    letter_soup = [
        ['A', 'M', 'A', 'R', 'S'],
        ['S', 'T', 'E', 'R', 'I'],
        ['O', 'M', 'A', 'Z', 'C'],
        ['A', 'L', 'C', 'R', 'H'],
        ['P', 'O', 'L', 'O', 'S']
    ]

    # Lista de palabras a buscar
    words = ["Amar", "Osa", "atar", "solo", "polos", "Mezclar", "Palo"]

    # Encontrar las palabras en la sopa de letras
    results = find_words(letter_soup, words)

    # Guardar los resultados en un archivo JSON
    output_path = "resultados.json"
    save_results_to_json(results, output_path)

    # Descargar el archivo generado
    from google.colab import files
    files.download(output_path)

    print("El archivo resultados.json ha sido descargado con los siguientes resultados:")
    print(json.dumps(results, indent=4))

if __name__ == "__main__":
    main()
