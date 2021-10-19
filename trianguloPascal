.data
saludo1: .asciiz "\n Triangulo de pascal"
saludo2: .asciiz "\n Cuantas lineas deseas ver: "
espacio: .asciiz " "
linea: .asciiz "\n"

.text
main:	li $v0 4 		# $a0 = 4  Ubiccion en la hoja de MIPS
	la $a0 saludo1
	syscall
	la $a0 saludo2
	syscall
	li $v0 5 		# $a1 = 5 Ubiccion en la hoja de MIPS Guarda el numero de lineas dadas por el usuario en v0
	syscall
	# registro para pascal
	move $t0 $v0 		# cantidad de lineas pedidos
	li $t1 0 		# cantidad de lineas mostradas
	li $t7 0 		# contador de numeros de la linea
	# registro para combinacion
	li $t2 0 		# numero de combinacion r y/o combinacion total
	li $t3 0 		# numero de combinacion k
	li $t4 0 		# numero temporal que guarda $t2-t3 para combinacion (r - k)
	# registro para factorial
	li $t5 0 		# numero actual del factorial
	li $t6 0 		# $t6-1

pascal_lineas:
	beq $t0 $t1 exit 	# Si cantidad de lineas pedidas = mostradas Salta a exit
	b actual_linea

sumar_linea:
	addi $t1 $t1 1 		# sumar +1 a lineas mostradas
	li $t7 0 		# resetar el registro de numeros mostrados por linea
	li $v0 4
	la $a0 linea 		# Salto de linea
	syscall
	b pascal_lineas
	
actual_linea:
	bgt $t7 $t1 sumar_linea #Si numero de linea < numeros mostrados en la linea Salta a sumar_linea
	move $t2 $t1 		# $t2 sera el numero de lineas
	move $t3 $t7 		# $t3 sera el contador de numeros mostrados por linea
	jal combinacion
	move $a0 $t2 		# $t2 es donde esta el resultado de la combinacion
	li $v0 1		#llama el valor de $a0 en $v0
	syscall			#imprime el resultado		
	li $v0 4
	la $a0 espacio		
	syscall			#imprime un espacio
	addi $t7 $t7 1 		# Sumar 1 a numeros mostrados por la linea actual
	b actual_linea

# (r!)/(r-k)!(k!) = (t2!)/($t2-$t3)!($t3!)
combinacion:
	move $s1 $ra
	sub $t4 $t2 $t3 	# $t4 = r-k
	move $t5 $t4 		# $t5 = r-k 
	jal factorial
	move $t4 $t5 		# $t4 = (r-k)! guarda el valor del factorial de (r-k) en $t4
	move $t5 $t3 		# guarda el valor de k en $t5, para pasarlo a la funcion factorial
	jal factorial
	move $t3 $t5 		# $t3 = k!
	move $t5 $t2		# guarda el valor de r en $t5 para pasarlo a la funcion factorial
	jal factorial
	move $t2 $t5 		# $t2 = r!
	mul $t4 $t4 $t3		# $t4 = (r-k)! * k!
	beqz $t2 combinacion_cero # Si r! = 0 division por 0
	beqz $t4 combinacion_cero # Si (r-k)! = 0 division por 0
	div $t2 $t2 $t4		# $t2 = r! / ((r-k)! * k!)
	b combinacion_return	# salta
	
combinacion_cero:
	li $t2 1
	b combinacion_return
	
combinacion_return: 
	jr $s1

factorial:
	move $t6 $t5 		#t6  = (r-k) EN la primera vez, luoego pasa a ser k y por ultimo r, para asi sacar el factorial de todos
	subi $t6 $t6 1 		#(r-k) - 1
	beqz $t5 factorial_cero # division por 0 (factorial de 0)
	b factorial_loop
	
factorial_loop:
	beqz $t6 factorial_return # termino si(r-k) = 0
	mul $t5 $t5 $t6 	# (r-k)*((r-k)-1)
	subi $t6 $t6 1 		#(r-k) - 1
	b factorial_loop
	
factorial_cero:
	li $t5 1
	b factorial_return
	
factorial_return: 
	jr $ra
	
exit:	li $v0 10
	syscall
