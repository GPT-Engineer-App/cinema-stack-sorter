# cinema-stack-sorter

#include "Stack.h"

elem* top = nullptr;
int lk = 0;

bool compare(cinema* a, cinema* b, int sort, bool u) {
    bool result;
    switch (u) {
    case 1: result = a->name < b->name; break;
    case 2: result = a->city < b->city; break;
    case 3: result = a->films < b->films; break;
    case 4: result = a->address < b->address; break;
    case 5: result = a->admin < b->admin; break;
    default: result = false;
    }
    return u ? result : !result;
}

void insertionSort(int sort, bool u) {
    if (top == nullptr || top->next == nullptr) {
        cout << "Стек пуст или содержит только один элемент, сортировка не требуется" << endl;
        return;
    }

    elem* sorted = nullptr;

    while (top != nullptr) {
        elem* current = top;
        top = top->next;
        lk--;
        elem* insertPos = sorted;
        elem* insertPosPrev = nullptr;
        while (insertPos != nullptr && compare(insertPos->minsk, current->minsk, sort, u)) {
            insertPosPrev = insertPos;
            insertPos = insertPos->next;
        }
        if (insertPosPrev == nullptr) {
            current->next = sorted;
            sorted = current;
        }
        else {
            current->next = insertPosPrev->next;
            insertPosPrev->next = current;
        }
    }
    top = sorted;
    while (sorted != nullptr) {
        lk++;
        sorted = sorted->next;
    }
}

elem* linearSearch(int sort, string value)     
{
    elem* current = top;
    while (current != nullptr) 
    {
        switch (sort) 
        {
            case 1: if (current->minsk->name == value) return current; break;
            case 2: if (current->minsk->city == value) return current; break;
            case 3: if (to_string(current->minsk->films) == value) return current; break;
            case 4: if (current->minsk->address == value) return current; break;
            case 5: if (current->minsk->admin == value) return current; break;
            default: break;
        }

        current = current->next;
    }

    return nullptr;
}

elem* popStack()
{
    if (top == nullptr)
    {
        cout << "Стек пустой" << endl;
        return NULL;
    }

    elem* pnt = top;
    top = top->next;
    lk--;

    return pnt;
}


void push(elem** top, cinema* Minsk) {
    elem* new_elem = new elem;
    new_elem->minsk = Minsk;
    new_elem->next = *top;
    *top = new_elem;
    lk++;
}

void print(elem* top) {
    while (top != nullptr) {

        cout << "===========================================" << endl;
        cout << "Название кинотеатра: " << top->minsk->name << endl;
        cout << "Город: " << top->minsk->city << endl;
        cout << "Кол-во фильмов " << top->minsk->films << endl;
        cout << "Адрес: " << top->minsk->address << endl;
        cout << "ФИО директора: " << top->minsk->admin << endl;
        cout << "===========================================" << endl;

        
        top = top->next;
    }
}

void printTable(elem* top) {

    cout << "--------------------------------------------------------------------------------------------------------------------\n";
    cout << "| Название кинотеатра      | Город            | Кол-во фильмов        | Адрес       | ФИО директора          |\n";
    cout << "--------------------------------------------------------------------------------------------------------------------\n";

    while (top != nullptr) {
        cout << "| " << setw(20) << left << top->minsk->name << " | ";
        cout << setw(20) << left << top->minsk->city << " | ";
        cout << setw(20) << left << top->minsk->films << " | ";
        cout << setw(20) << left << top->minsk->address << " | ";
        cout << setw(20) << left << top->minsk->admin << " |\n";
        cout << "--------------------------------------------------------------------------------------------------------------------\n";
        top = top->next;
    }
}

void deleteStack()
{
    while (top != nullptr)
    {
        popStack();
    }
    cout << "Стек удален" << endl;
}

void isStackEmpty()
{
    if (lk == 0)
    {
        cout << "Стек пустой..." << endl;
    }
    else
    {
        cout << "Стек заполнен..." << endl;
    }
}

void deleteByIndex(int index)
{
    if (index < 0 || index >= lk)
    {
        cout << "Индекс вне диапазона." << endl;
        return;
    }

    elem* current = top;
    elem* prev = nullptr;

    for (int i = 0; i < index; ++i)
    {
        prev = current;
        current = current->next;
    }

    if (prev != nullptr)
    {
        prev->next = current->next;
    }
    else
    {
        top = current->next;
    }

    //delete current->minsk;
    delete current;
    lk--;
}



void StackSwap() {

    int index1, index2;

    cout << "Введите элемент который хотите поменять: ";
    cin >> index1;

    cout << "Введите с каким элементом поменять: ";
    cin >> index2;

    if (index1 < 0 || index1 >= lk || index2 < 0 || index2 >= lk || index1 == index2) {
        cout << "Один из индексов вне диапазона или индексы равны." << endl;
        return;
    }

    elem* current1 = top;
    elem* prev1 = nullptr;
    elem* current2 = top;
    elem* prev2 = nullptr;

    // Находим первый элемент
    for (int i = 0; i < index1; ++i) {
        prev1 = current1;
        current1 = current1->next;
    }

    // Находим второй элемент
    for (int i = 0; i < index2; ++i) {
        prev2 = current2;
        current2 = current2->next;
    }

    // Если один из элементов является вершиной стека
    if (!prev1 || !prev2) {
        if (index1 == 0) {
            top = current2;
        }
        else {
            top = current1;
        }
    }

    if (prev1) prev1->next = current2;
    if (prev2) prev2->next = current1;

    elem* temp = current1->next;
    current1->next = current2->next;
    current2->next = temp;
}

bool modifyByIndex(elem* top, int index, int& field,string& newValue) {
    int currentIndex = 0;
    while (top != nullptr && currentIndex < index) {
        top = top->next;
        currentIndex++;
    }

    if (top == nullptr) {
        cout << "Элемент с индексом " << index << " не найден." << endl;
        return false;
    }

    if (field == 1) {
        top->minsk->name = newValue;
    }
    else if (field == 2) {
        top->minsk->city = newValue;
    }
    else if (field == 3) {
        top->minsk->films = stoi(newValue);

    }
    else if (field == 4) {
        top->minsk->address = newValue;

    }
    else if (field == 5) {
        top->minsk->admin = newValue;
    }
    else {
        cout << "Неверное номер поля: " << field << endl;
        return false;
    }
    return true;
}

bool replaceByIndex(elem** top, int index, const cinema& newCinema) {
    int currentIndex = 0;
    elem* current = *top;
    elem* prev = nullptr;

    while (current != nullptr && currentIndex < index) {
        prev = current;
        current = current->next;
        currentIndex++;
    }

    if (current == nullptr) {
        cout << "Элемент с индексом " << index << " не найден." << endl;
        return false;
    }

    current->minsk = new cinema(newCinema);
    return true;
}
Добавь к этому коду бинарный поиск

## Collaborate with GPT Engineer

This is a [gptengineer.app](https://gptengineer.app)-synced repository 🌟🤖

Changes made via gptengineer.app will be committed to this repo.

If you clone this repo and push changes, you will have them reflected in the GPT Engineer UI.

## Tech stack

This project is built with React and Chakra UI.

- Vite
- React
- Chakra UI

## Setup

```sh
git clone https://github.com/GPT-Engineer-App/cinema-stack-sorter.git
cd cinema-stack-sorter
npm i
```

```sh
npm run dev
```

This will run a dev server with auto reloading and an instant preview.

## Requirements

- Node.js & npm - [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating)
