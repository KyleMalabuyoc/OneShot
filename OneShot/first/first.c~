#include<stdio.h>
#include<stdlib.h>

double** transpose(double** X, int tempsize, int size1);

double** transpose(double** X, int tempsize, int size1){

	int i = 0, j = 0;

	double** newX; //build Y vector of prices
	newX = (double**)malloc(tempsize*sizeof(double*));

	for(i = 0; i < tempsize; i++){
		newX[i] = (double *)malloc(size1*sizeof(double));
	}

	for(i = 0; i < size1; i++){

		for(j = 0; j < tempsize; j++){
	
			newX[j][i] = X[i][j];

		}
	}

	return newX;
}

int main(int argc, char** argv){

	if(argc < 3){//change to 3
		printf("error\n");
		return 0;
	}
	FILE *fp = fopen(argv[1], "r");

	if(fp == NULL){
		printf("error\n");
		return 0;
	}
		int size1, size2;
		fscanf(fp, "%d\n%d\n", &size2, &size1); //matrix dimensions

		int tempsize = size2;
		size2 = size2+1;
	
		double** matrix;
		matrix = (double**)malloc(size1*sizeof(double*));
		int i,j;	

	for(i = 0; i < size1; i++){
		matrix[i] = (double *)malloc(size2*sizeof(double));
	}
			for(i = 0; i < size1; i++){
			
				for(j = 0; j < size2; j++){
					if(!fscanf(fp, "%lf,", &matrix[i][j])){
						break;		
				}
			}	
		}

	fclose(fp);
	
	fp = fopen(argv[2], "r"); //test file

	if(fp == NULL){
		printf("error\n");
		return 0;
	}
		int size3;
		fscanf(fp, "%d\n", &size3); //matrix dimensions
		
		//printf("%d %d",size1,size2);
	
		double** Xfin;
		Xfin = (double**)malloc(size3*sizeof(double*));
			

	for(i = 0; i < size3; i++){
		Xfin[i] = (double *)malloc(tempsize*sizeof(double));
	}
			for(i = 0; i < size3; i++){
			
				for(j = 0; j < tempsize; j++){
					if(!fscanf(fp, "%lf,", &Xfin[i][j])){
						break;		
				}
			}	
		}

	fclose(fp);
////////////////////////////////////////////////////////////////////build X matrix of attributes
	double** X; 
	X = (double**)malloc(size1*sizeof(double*));

	for(i = 0; i < size1; i++){
		X[i] = (double *)malloc(size2*sizeof(double));
	}

	for(i = 0; i < size1; i++){
		X[i][0] = 1;
	}
		int w = 0, x = 0;
		
	for(i = 0; i < size1; i++){

		for(j = 1; j < size2; j++){
			
			X[i][j] = matrix[w][x];	
			x++;		
		}
		w++;
		x = 0;
	}

	double** Xtest; 
	Xtest = (double**)malloc(size3*sizeof(double*));

	for(i = 0; i < size3; i++){
		Xtest[i] = (double*)malloc(size2*sizeof(double));
	}

	for(i = 0; i < size3; i++){
		Xtest[i][0] = 1;
	}
		w = 0, x = 0;
		
	for(i = 0; i < size3; i++){

		for(j = 1; j < size2; j++){
			
			Xtest[i][j] = Xfin[w][x];	
			x++;		
		}
		w++;
		x = 0;
	}
/////////////////////////////////////////////////////////////////////build Y vector of prices
	double** Y; 
	Y = (double**)malloc(size1*sizeof(double*));

	for(i = 0; i < size1; i++){
		Y[i] = (double *)malloc(size1*sizeof(double));
	}

	for(i = 0; i < size1; i++){
			Y[i][0] = matrix[i][tempsize];		
	}
/////////////////////////////////////////////////////////////////////build X transpose
	double** xTrans; 
	xTrans = (double**)malloc(tempsize*sizeof(double*));

	for(i = 0; i < tempsize; i++){
		xTrans[i] = (double *)malloc(size1*sizeof(double));
	}

	xTrans = transpose(X,size2,size1);
///////////////////////////////////////////////////////////////////matrix multiplication of X and X transpose 				
	double** result;
	result = (double**)malloc(size2*sizeof(double*));

	for(i = 0; i < size2; i++){
		result[i] = (double *)malloc(size2*sizeof(double));
	}
	int a,b,c;
	double sum;

	for(a = 0; a < size2; a++){
			
				for(b = 0; b < size2; b++){  
					
					for(c = 0; c < size1; c++){
						double product = xTrans[a][c]*X[c][b];
						sum = sum + product;
					}
			result[a][b] = sum;
			sum = 0; //reset sum for new location in matrix
			}	
	}
///////////////////////////////////////////////////////////////////identity matrix
	double** inverse; 
	inverse =  (double**)malloc(size2*sizeof(double*));

	for(i = 0; i < size2; i++){
		inverse[i] = (double *)malloc(size2*sizeof(double));
	}

	for(i = 0; i < size2; i++){

			inverse[i][i] = 1;//initialize 
	}
/////////////////////////////////////////////////////////////////////Gauss Jordan Elimination 
	int z;
	double v;
	int indexCheck;

	for(i=0; i < size2; i++){
	
		for(j=0; j < size2; j++){

			if(j == i){
				indexCheck = 0;
			}else{
				indexCheck = 1;
			}

			if(indexCheck == 1){

			if(result[i][i] != 1){

				int col;
				double val = result[i][i];
			
			for(col = 0; col < size2; col++){ //change diagonal values to 1
				result[i][col] = result[i][col]/val;
				inverse[i][col] = inverse[i][col]/val; //divide indentity values with leading coeff in result
			}

		}
		
		v = (1/(result[i][i]))*result[j][i]; //multiple with current
 
		for(z = 0; z < size2; z++){

			result[j][z] = result[j][z] - (result[i][z]*v);
			inverse[j][z] = inverse[j][z] - (inverse[i][z]*v); //perform same operation on identity
                }
		//printf("%lf ",c);
            }
        }
    }
///////////////////////////////////////////////////////////////////XTRANSPOSE TIMES Y VECTOR
	double** Yprod;
	Yprod = (double**)malloc(size2*sizeof(double*));

	for(i = 0; i < size2; i++){
		Yprod[i] = (double *)malloc(1*sizeof(double));
	}

	for(a = 0; a < size2; a++){
			
				for(b = 0; b < 1; b++){  
					
					for(c = 0; c < size1; c++){
						double product = xTrans[a][c]*Y[c][b];
						sum = sum + product;
					}
			Yprod[a][b] = sum;
			sum = 0; //reset sum for new location in matrix
			}	
	}
/////////////////////////////////////////////////////////////////////////INVERSE TIMES (XTRANS*Y)
	double** W;
	W = (double**)malloc(size2*sizeof(double*));

	for(i = 0; i < size2; i++){
		W[i] = (double *)malloc(1*sizeof(double));
	}

	for(a = 0; a < size2; a++){
			
				for(b = 0; b < 1; b++){  
					
					for(c = 0; c < size2; c++){
						double product = inverse[a][c]*Yprod[c][b];
						sum = sum + product;
					}
			W[a][b] = sum;
			sum = 0; //reset sum for new location in matrix
			}	
	}
///////////////////////////////////////////////////////////////////////
	double** final;
	final = (double**)malloc(size3*sizeof(double*));

	for(i = 0; i < size3; i++){
		final[i] = (double *)malloc(1*sizeof(double));
	}

	for(a = 0; a < size3; a++){
			
				for(b = 0; b < 1; b++){  
					
					for(c = 0; c < size2; c++){
						double product = Xtest[a][c]*W[c][b];
						sum = sum + product;
					}
			final[a][b] = sum;
			sum = 0; //reset sum for new location in matrix
			}	
	}
///////////////////////////////////////////////////////////////////printing method
	for(i = 0; i < size3; i++){ //print xTrans*X

			printf("%0.0f\n",final[i][0]);
		
	}
	return 0;
}
