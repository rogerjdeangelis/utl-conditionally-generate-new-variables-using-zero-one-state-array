# utl-conditionally-generate-new-variables-using-zero-one-state-array
Conditionally generate new variables using zero one state array

    Conditionally generate new variables using zero one state array;

    github
    https://tinyurl.com/ydbpzlbo
    https://github.com/rogerjdeangelis/utl-conditionally-generate-new-variables-using-zero-one-state-array

    sas forum
    https://tinyurl.com/y7vr7qpw
    https://communities.sas.com/t5/SAS-Programming/how-to-rename-variable-names-in-batch-maybe-using-array/m-p/511815


    INPUT
    =====

    Middle Observation(1 ) of have - Total Obs 1

    Variable Value   Variable Value |  RULES
    -------- -----   -------- ----- |  -----

      A        9      X1         1  |  since x1=1 create A_A=3
      B        9      X2         0  |  do not generate B_B
                                    |
      C        9      X3         1  |
      D        9      X4         0  |
                                    |
      E        9      X5         1  |  since x5=1 create E_E=3
      F        9      X6         0  |  do not generate F_F
                                    |
      G        9      X7         1  |  ...
      H        9      X8         0  |
      I        9      X9         1  |
      J        9      X10        0  |
      K        9      X11        1  |
      L        9      X12        0  |
      M        9      X13        1  |
      N        9      X14        0  |
      O        9      X15        1  |
      P        9      X16        0  |
      Q        9      X17        1  |
      R        9      X18        0  |
      S        9      X19        1  |
      T        9      X20        0  |
      U        9      X21        1  |
      V        9      X22        0  |
      W        9      X23        1  |
      X        9      X24        0  | ...
                                    |
      Y        9      X25        1  | since x25=1 create Y_Y=3
      Z        9      X26        0  | do not generate Z_Z


    EXAMPLE OUTPUT
    --------------

    Middle Observation(1 ) of have - Total Obs 1

    Variable Value   Variable Value
    -------- -----   -------- -----

      A        9       A_A      3
      B        9
      C        9       C_C      3
      D        9
      E        9       E_E      3
      F        9
      G        9       G_G      3
      H        9
      I        9       I_I      3
      J        9
      K        9       K_K      3
      L        9
      M        9       M_M      3
      N        9
      O        9       O_O      3
      P        9
      Q        9       Q_Q      3
      R        9
      S        9       S_S      3
      T        9
      U        9       U_U      3
      V        9
      W        9       W_W      3
      X        9
      Y        9       Y_Y      3
      Z        9


    PROCESS
    =======

    %symdel varAdd / nowarn;
    data want;

      * build the meta data;
      if _n_=0 then do; %let rc=%sysfunc(dosubl('

          data _null_;

            length varAdd $400;
            set have;

            array ltrs[*] &letters;
            array logic[*] x1-x26;

            do idx=1 to dim(ltrs);
               if logic[idx]=1 then varAdd=cats(varAdd,vname(ltrs[idx]),'_',vname(ltrs[idx]),'=3;' );
            end;

            call symputx('varAdd',varAdd);

          run;quit;
          '));

      end;

      array ltrs[*] &letters (26*9);
      &varAdd;

    run;quit;

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    * letters macro variable is in my autoexec;;
    %let letters=A B C D E F G H I J K L M N O P Q R S T U V W X Y Z;
    %let logic=  1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0 1 0;

    data have;
      array ltrs[*] &letters (26*9);
      array logic[*] x1-x26 (&logic);
    run;quit;




