package edu.ust.cisc;

import java.util.Iterator;
import java.util.Arrays;
import java.util.Objects;
import java.io.*;

public class CiscArrayList<E> implements CiscList<E> {

    private static final int DEFAULT_CAPACITY = 10;
    private E[] elementData;
    private int size;

    public CiscArrayList() {
        this.elementData = (E[]) new Object[DEFAULT_CAPACITY];
    }

    public CiscArrayList(int initialCapacity) {
        if (initialCapacity < 0) {
            throw new IllegalArgumentException();
        }
        this.elementData = (E[]) new Object[initialCapacity];
        this.size = 0;
    }

    @Override
    public int size() {
        return this.size;
    }

    @Override
    public boolean isEmpty() {
        return this.size == 0;
    }

    @Override
    public boolean contains(Object o) {
        for (int i = 0; i < size; i++) {
            if (elementData[i].equals(o)) {
                return true;
            }
        }
        return false;
    }

    @Override
    public Object[] toArray() {
        E[] arrayobj = Arrays.copyOf(this.elementData,this.size);
        return arrayobj;
    }

    @Override
    public boolean add(E e) {
        this.ensureCapacity(this.size + 1);
        int i;
        for(i = 0; i < this.size; i++){
            if(this.elementData[i]!=null) {
                if(i == (this.size-1)){
                    return false;
                }
            }
            else{
                this.elementData[i] = e;
                return true;
            }
        }
        return false;
    }

    @Override
    public Iterator<E> iterator() {
        throw new UnsupportedOperationException();
    }

    @Override
    public boolean remove(Object o) {
        for(int i = 0; i<size; i++) {
            if (Objects.equals(o, get(i))) {
                for (int j = i; j + 1 < size; j++) {
                    elementData[j] = elementData[j + 1];
                }
                elementData[size - 1] = null;
                size = size-1;
                return true;
            }
        }
        return false;
    }


    @Override
    public void clear() {
        for(int i = 0; i<size; i++){
            elementData[i] = null;
        }
        size = 0;
    }

    @Override
    public E get(int index) {
        if (index < size) {
            return elementData[index];
        } else {
            throw new IndexOutOfBoundsException();
        }
    }

    @Override
    public E set(int index, E element) {
        if (index < size) {
            E newElement = this.elementData[index];
            this.elementData[index] = element;
            return newElement;
        } else {
            throw new IndexOutOfBoundsException();
        }
    }

    @Override
    public void add(int index, E element) {
//shift over to the right to add space
       if(index > size || index < 0){
           throw new IndexOutOfBoundsException();
       }
        for(int i = size; i>index; i--){
           elementData[i] = elementData[i - 1];
       }
        elementData[index] = element;
        size = size + 1;
    }

    @Override
    public E remove(int index) {
        if(index > size || index < 0){
            throw new IndexOutOfBoundsException();
        }
        E element = elementData[index];
        for(int i = index; i + 1 < size; i++){
            elementData[i] = elementData[i+1];
        }
        elementData[size-1] = null;
        size = size - 1;
        return element;
    }

    @Override
    public int indexOf(Object o) {
        for (int i = 0; i < size; i++) {
            if (elementData[i].equals(o)) {
                return i;
            }
        }
        return -1;
    }

    public void ensureCapacity(int minimumCapacity) {
        //should ask two potential questions. Do we need to increase capacity? if yes, what should it be?
        for(int i = 0; i < this.size; i++) {
            if (this.elementData[i] == null) {
                return;
            }
        }
        if (this.size < minimumCapacity) {
            int newsize;
            if(this.size == 0) {
                this.size = 1;
            }
            else if (this.size * 2 + 1 < minimumCapacity) {
                newsize = minimumCapacity;
                E[] newarray = Arrays.copyOf(this.elementData, newsize);
                this.size = newsize;
                this.elementData = newarray;
            }
            else {
                newsize = this.size * 2 + 1;
                E[] newarray = Arrays.copyOf(this.elementData, newsize);
                this.size += 1;
                this.elementData = newarray;
            }
        }
    }
}
